namespace Last_Laba
{
    // товары
    class Product
    {
        public int ProductId { get; set; }
        public string ProductName { get; set; }
        public Product(int id, string name)
        {
            ProductId = id;
            ProductName = name;
        }
    }
    // поставка
    class Delivery
    {
        public int ProviderId { get; set; }
        public int ProductId { get; set; }
        public DateTime Date { get; set; }
        public int Count { get; set; }
        public Delivery(int provider_id, int product_id, DateTime date, int count) // от кого, что, когда и сколько
        {
            ProductId = product_id;
            ProviderId = provider_id;
            Date = date;
            Count = count;
        }
    }
    // поставщик
    class Provider
    {
        public int ProviderId { get; set; }
        public string CompanyName { get; set; }
        public string CompanyPhone { get; set; }
        public Provider(int id, string name, string phone)
        {
            ProviderId = id;
            CompanyName = name;
            CompanyPhone = phone;
        }

    }
    internal class Program
    {
        static void Main(string[] args)
        {
            Product[] products =
            {
                new Product(1001, "Молоко"),
                new Product(1002, "Хлебцы"),
                new Product(1003, "Пастила"),
                new Product(1004, "Яйца"),
                new Product(1005, "Крабовые палочки")
            };
            Provider[] providers =
            {
                new Provider(1,"Пятерочка", "8-(900)-234-76-45"),
                new Provider(2,"Магнит", "8-(913)-630-27-83"),
                new Provider(3,"Победа", "8-(616)-111-33-11")
            };
            Delivery[] deliveries =
            {
                new Delivery(1, 1004, new DateTime(2023, 6, 1), 20),
                new Delivery(1, 1002, new DateTime(2023, 5, 31), 100),
                new Delivery(1, 1001, new DateTime(2023, 6, 2), 11),

                new Delivery(2, 1005, new DateTime(2023, 5, 30), 89),
                new Delivery(2, 1001, new DateTime(2023, 6, 1), 77),

                new Delivery(3, 1003, new DateTime(2023, 5, 30), 266)

            };
            // 1) Сделать
            // По дате поставки товаров: кто поставил, что и в каком количестве
            Console.WriteLine($"\t\t\t\t - Запрос 1 -");
            var response1 = from deliver in deliveries
                            orderby deliver.Date

                            group deliver by deliver.Date into dlvr
                            select new
                            {
                                Name = dlvr.Key,
                                Count = dlvr.Count(),
                                Employes = from p in dlvr
                                           join product in products
                                           on p.ProductId equals product.ProductId

                                           join provider in providers
                                           on p.ProviderId equals provider.ProviderId

                                           select new
                                           {
                                               eProduct = product.ProductName,
                                               eProvider1 = provider.CompanyName,
                                               eProvider2 = provider.CompanyPhone
                                           }
                            };
            foreach (var r1 in response1)
            {
                Console.WriteLine($"{r1.Name.ToString("MM/dd/yyyy")} - {r1.Count}");

                foreach (var a in r1.Employes)
                {
                    Console.WriteLine($"Наименование товара: {a.eProduct}");
                    Console.WriteLine($"Поставщик: {a.eProvider1}, (контактный телефон - {a.eProvider2})");
                }
                Console.WriteLine(); // для разделения между группами
            }

            // 2) Сгруппировать по товару
            // Кто поставщики данного товара
            Console.WriteLine($"\t\t\t\t - Запрос 2 -");
            var response2 = from prod in products
                        select new
                        {
                            Name = prod.ProductName,
                            Companies = from delivery in deliveries
                                        where prod.ProductId == delivery.ProductId
                                        join provider in providers
                                        on delivery.ProviderId equals provider.ProviderId
                                        select new
                                        {
                                            Name = provider.CompanyName,
                                            Phone = provider.CompanyPhone
                                        }
                        };
            foreach (var r2 in response2)
            {
                Console.WriteLine($"Товар: {r2.Name}");
                foreach (var a in r2.Companies)
                    Console.WriteLine($"Поставщик {a.Name}, (контактный телефон - {a.Phone})");
                Console.WriteLine();
            }
            // 3) Сгруппировать по поставщику
            // Поставщик и какие товары он поставлял
            Console.WriteLine($"\t\t\t\t - Запрос 3 -");
            var response3 = from provider in providers
                            select new
                            {
                                Name = provider.CompanyName,
                                Products = from delivery in deliveries
                                            where provider.ProviderId == delivery.ProviderId
                                            join product in products
                                            on delivery.ProductId equals product.ProductId
                                            select product.ProductName
                            };
            foreach (var r3 in response3)
            {
                Console.WriteLine($"Поставщик: {r3.Name}");
                foreach (var b in r3.Products)
                    Console.WriteLine($"Товар: {b}");
                Console.WriteLine();
            }

        }
    }
}
