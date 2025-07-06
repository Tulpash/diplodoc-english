# RabbitMQ в .NET
{% note info %}

dotnet add package RabbitMQ.Client --version 6.8.1

{% endnote %}

## Startup
```cs
using Microsoft.AspNetCore.Builder;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using RabbitMQ.Client;
using RabbitMQExample.Services;

var builder = WebApplication.CreateBuilder(args);

// Настройка RabbitMQ
builder.Services.AddSingleton<IConnectionFactory>(sp => new ConnectionFactory
{
    HostName = "localhost",
    Port = 5672,
    UserName = "guest",
    Password = "guest",
    DispatchConsumersAsync = true // Для асинхронной обработки
});

// Регистрация сервисов
builder.Services.AddControllers();
builder.Services.AddHostedService<RabbitMQConsumer>();

var app = builder.Build();

app.UseRouting();
app.UseEndpoints(endpoints => endpoints.MapControllers());

app.Run();
```

## Производитель
```cs
using Microsoft.AspNetCore.Mvc;
using RabbitMQ.Client;
using System.Text;
using System.Text.Json;

namespace RabbitMQExample.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    public class MessageController : ControllerBase
    {
        private readonly IConnectionFactory _connectionFactory;

        public MessageController(IConnectionFactory connectionFactory)
        {
            _connectionFactory = connectionFactory;
        }

        [HttpPost]
        public IActionResult SendMessage([FromBody] string message)
        {
            // Создаем соединение и канал
            using var connection = _connectionFactory.CreateConnection();
            using var channel = connection.CreateModel();

            // Объявляем обменник (direct)
            channel.ExchangeDeclare("my_exchange", ExchangeType.Direct, durable: true);

            // Объявляем очередь
            channel.QueueDeclare(queue: "my_queue", durable: true, exclusive: false, autoDelete: false, arguments: null);

            // Привязываем очередь к обменнику
            channel.QueueBind(queue: "my_queue", exchange: "my_exchange", routingKey: "my_routing_key");

            // Сериализуем сообщение
            var messageBody = JsonSerializer.Serialize(new { Message = message, Timestamp = DateTime.UtcNow });
            var body = Encoding.UTF8.GetBytes(messageBody);

            // Публикуем сообщение
            channel.BasicPublish(exchange: "my_exchange", routingKey: "my_routing_key", basicProperties: null, body: body);

            return Ok("Message sent to RabbitMQ");
        }
    }
}
```

## Потребитель
```cs
using Microsoft.Extensions.Hosting;
using RabbitMQ.Client;
using RabbitMQ.Client.Events;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

namespace RabbitMQExample.Services
{
    public class RabbitMQConsumer : BackgroundService
    {
        private readonly IConnectionFactory _connectionFactory;

        public RabbitMQConsumer(IConnectionFactory connectionFactory)
        {
            _connectionFactory = connectionFactory;
        }

        protected override Task ExecuteAsync(CancellationToken stoppingToken)
        {
            stoppingToken.ThrowIfCancellationRequested();

            // Создаем соединение и канал
            var connection = _connectionFactory.CreateConnection();
            var channel = connection.CreateModel();

            // Объявляем обменник и очередь
            channel.ExchangeDeclare("my_exchange", ExchangeType.Direct, durable: true);
            channel.QueueDeclare(queue: "my_queue", durable: true, exclusive: false, autoDelete: false, arguments: null);
            channel.QueueBind(queue: "my_queue", exchange: "my_exchange", routingKey: "my_routing_key");

            // Настраиваем QoS (максимум 1 сообщение за раз)
            channel.BasicQos(prefetchSize: 0, prefetchCount: 1, global: false);

            // Создаем потребителя
            var consumer = new EventingBasicConsumer(channel);
            consumer.Received += (model, ea) =>
            {
                var body = ea.Body.ToArray();
                var message = Encoding.UTF8.GetString(body);
                Console.WriteLine($"Received message: {message}");

                // Подтверждаем обработку
                channel.BasicAck(deliveryTag: ea.DeliveryTag, multiple: false);
            };

            // Подписываемся на очередь
            channel.BasicConsume(queue: "my_queue", autoAck: false, consumer: consumer);

            return Task.CompletedTask;
        }
    }
}
```

{% note alert %}

Не стоит создавать каналы на каждый отдельный запрос, канал леговесный объект внутри соединения (соединение внутри соединения), работать с ним стоит так же как с соединением, один раз содать и держать открытым.

{% endnote %}

{% note alert %}

Каналы не потокобезопасны

{% endnote %}