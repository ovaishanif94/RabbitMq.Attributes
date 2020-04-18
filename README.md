# RabbitMq.Attributes

This package helps you to build messaging job on attributes.

### Download
The latest stable release is available on NuGet: https://www.nuget.org/packages/RabbitMq.Attributes/

`Install-Package RabbitMq.Attributes -Version 1.0.1`

#### Example:

**Listener**

Program.cs

    public class Program
    {
        public static void Main(string[] args)
        {
            IConfiguration configuration = new ConfigurationBuilder()
                .AddJsonFile("appsettings.json")
                .Build();
            var services = new ServiceCollection();

            // add logging
            services.AddLogging(x => x.AddConsole());

            // add rabbit mq 
            services.AddRabbitMqManager(x =>
            {
                x.Username = "guest";
                x.Password = "guest";
                x.VirtualHost = "/";
                x.HostName = "localhost";
                x.Port = 5672;
            });
            services.AddSingleton(provider => configuration);
            services.AddSingleton<QueueClass>();
            
            // start listener
            ServiceProvider serviceProvider = services.BuildServiceProvider();
            serviceProvider.StartQueueListener();
        }
    }

    
    [AMQueueTrigger]
    public class QueueClass
    {
        [AMQueue("test-queue1")]
        public async Task TestQueueAsync(Model model)
        {
            // code here
        }
        
        [AMQueue("test-queue2")]
        public async Task TestQueue2Async(Model2 model2)
        {
            // code here
        }
    }

**Sender**

Startup.cs

    // add rabbit mq 
    services.AddRabbitMqManager(x =>
    {
        x.Username = "guest";
        x.Password = "guest";
        x.VirtualHost = "/";
        x.HostName = "localhost";
        x.Port = 5672;
    });

    public class MyClass
    {
        public MyClass(IAMQueueStorage aMQueueStorage)
        {
            aMQueueStorage.Add("test-queue1", model);
        }
    }

    
# Buy me a coffee :coffee:

ETH: 0xC32Cce6e9A2C88fBe77430B94306C2Db443c06d4

BTC: 1MWVz2MFuHrnTfzxZ8GUSiZXgKuTuRHNLW

BCH: bitcoincash:qr8xav6nzhaj2l9xutvhps36sgxcn8nknge0jz567s
