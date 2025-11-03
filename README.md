using System;
using System.Collections.Generic;
namespace AgüeroGarcíaIntento1

{
    //struct para almacenar los datos de los video juegos
    struct videoJuego
    {
        public string nombre;
        public char categoria;
        public double precio;
        public int edadMinima;
        public bool disponible;

        public videoJuego(string nombre, char categoria, double precio, int edadMinima, bool disponible)
        {
            this.nombre = nombre;
            this.categoria = categoria;
            this.precio = precio;
            this.edadMinima = edadMinima;
            this.disponible = disponible;
        }

        public void mostrarInfo()
        {
            Console.WriteLine($"Nombre: {nombre}");
            Console.WriteLine($"Categoría: {categoria}");
            Console.WriteLine($"Precio: {precio}");
            Console.WriteLine($"Edad mínima: {edadMinima}");
            Console.WriteLine($"Disponible: {disponible}");
        }
    }


    internal class Program
    {
        //Lista que contiene a los juegos y sus caracteristicas
        static List<videoJuego> videoJuegos = new List<videoJuego>();
        //Arreglo del nombre de las empresas
        static string[] empresas =
            { "Desarrollos Fantásticos",
            "Gaming Studio",
            "Pixel Creations"
            };
        //Matriz para el registro de ventas en el año
        static int[,] ventas = new int[empresas.Length, 12];

        //Función para mostrar el Menú
        static void mostarMenu()
        {
            Console.WriteLine("----Menú----");
            Console.WriteLine("Ingrese la opción que desea utilizar");
            Console.WriteLine("1.Agregar un video juego ");
            Console.WriteLine("2. Buscar un video juego");
            Console.WriteLine("3. Lista los videos juegos");
            Console.WriteLine("4. Ordenar los video juegos por precio");
            Console.WriteLine("5. Registrar las ventas");
            Console.WriteLine("6. Calcular el promedio de precios");
            Console.WriteLine("7. Filtrar por edad mínima");
            Console.WriteLine("8. Salir");
        }

        static void agregarVideojuego()
        {
            char continuar;

            do
            {

                videoJuego nuevoJuego;
                bool existe = false;
                Console.WriteLine("\n---Agregar juego----");

                //Se solicita que ingrese el nombre del nuevo juego
                Console.WriteLine("Ingrese el nombre del nuevo video juego");
                nuevoJuego.nombre = Console.ReadLine();

                //Se recorre el arreglo con un foreach para verificar que el juego ya no este ingresado
                foreach (var juego in videoJuegos)
                {
                    if (juego.nombre.Equals(nuevoJuego.nombre, StringComparison.OrdinalIgnoreCase))
                    {
                        existe = true;
                        break;
                    }
                }

                if (existe)
                {

                    Console.WriteLine("El juego ya existe en la lista.");
                 
                }

                else
                {
                    //Categoría
                    Console.WriteLine("Ingrese la categoria (A/R/D");
                    nuevoJuego.categoria = char.ToUpper(Console.ReadKey().KeyChar);
                    Console.WriteLine();

                    //Precio
                    Console.WriteLine("Ingrese el precio del video juego");
                    while (!double.TryParse(Console.ReadLine(), out nuevoJuego.precio) || nuevoJuego.precio < 0) 
                    {
                        Console.WriteLine("Precio inválido. Ingrese un valor nuevamente.");
                    }

                    //Edad Mínima
                    Console.WriteLine("Ingrese la edad mínima");
                    while (!int.TryParse(Console.ReadLine(), out nuevoJuego.edadMinima) || nuevoJuego.edadMinima < 0)
                    {
                        Console.WriteLine("Edad inválida. Intente nuevamente");
                    }

                    Console.WriteLine("¿Está disponible? Ingrese ture o false");
                    while (!bool.TryParse(Console.ReadLine(), out nuevoJuego.disponible))
                    {
                        Console.WriteLine("Valor inválido. Intente nuevamente");
                    }
                    videoJuegos.Add(nuevoJuego);
                    Console.WriteLine("Videojuego agregado con éxito.");
                }

                Console.WriteLine("\n¿Desea agregar otro viedo juego?");
                continuar = char.ToUpper(Console.ReadKey().KeyChar);

            } while (continuar == 'S');

            Console.WriteLine("Presione cualquier tecla para volver al menú");
            Console.ReadKey();
        }
        //Función para buscar un videojuego
        static void buscarVideojuego()
        {
            Console.WriteLine("Ingrese el nombre del video juego que desea buscar");
            string nombreBuscado = Console.ReadLine();
            bool encontrado = false;

            foreach (var juego in videoJuegos)
            {
                if (juego.nombre.Equals(nombreBuscado, StringComparison.OrdinalIgnoreCase))
                {
                    Console.WriteLine($"Nombre {juego.nombre}, Categoria: {juego.categoria}, Precio: {juego.precio}, Edad mínima {juego.edadMinima}, Disponible {juego.disponible}");
                    encontrado = true;
                    break;
                }
            }

            if (!encontrado)
            {
                Console.WriteLine("No se pudo encontrar el videojuego ingresado.");
            }
        }

        //Función para poder listar a los videosjuegos

        static void listarVideojuegos()
        {
            Console.WriteLine("---Lista de Videojuegos---");

            foreach (var juego in videoJuegos)
            {
                Console.WriteLine($"Nombre{juego.nombre}, Categoria: {juego.categoria}, Precio: {juego.precio}, Edad mínima {juego.edadMinima}, Disponible {juego.disponible} ");
            }
        }

        //Función para ordenar los videojuegos por precio de forma ascedente. mediante burbujeo
        static void ordenarVideojuegosPorprecio()
        {
            for (int i = 0; i < videoJuegos.Count - 1; i++)
            {
                for (int j = 0; j < videoJuegos.Count - 1; j++)
                {
                    if (videoJuegos[j].precio > videoJuegos[j + 1].precio)
                    {
                        //Se intercambia el orden de los juegos
                        var temp = videoJuegos[j];
                        videoJuegos[j] = videoJuegos[j + 1];
                        videoJuegos[j + 1] = temp;
                    }
                }
            }

            Console.WriteLine("Videojuegos ordenados con éxito");
        }

        static void registrarVentas()
        {
            Console.WriteLine("\n--- Registrar y Calcular Ventas ---");

            // Mostrar empresas
            for (int i = 0; i < empresas.Length; i++)
            {
                Console.WriteLine($"{i + 1}. {empresas[i]}");
            }

            // Seleccionar empresa
            Console.Write("\nSeleccione una empresa (1-3): ");
            int indiceEmpresa = int.Parse(Console.ReadLine()) - 1;

            if (indiceEmpresa < 0 || indiceEmpresa >= empresas.Length)
            {
                Console.WriteLine(" Empresa no válida.");
                return;
            }

            // Registrar ventas mensuales
            Console.WriteLine($"\nRegistrando ventas para: {empresas[indiceEmpresa]}");
            for (int mes = 0; mes < 12; mes++)
            {
                Console.Write($"Ventas del mes {mes + 1}: ");
                ventas[indiceEmpresa, mes] = int.Parse(Console.ReadLine());
            }

            Console.WriteLine("\n Ventas registradas.");

            // Calcular ventas anuales
            int[] ventasAnuales = new int[empresas.Length];

            for (int i = 0; i < empresas.Length; i++)
            {
                for (int mes = 0; mes < 12; mes++)
                {
                    ventasAnuales[i] += ventas[i, mes];
                }
            }

            // Mostrar antes de ordenar
            Console.WriteLine("\nVentas anuales:");
            for (int i = 0; i < empresas.Length; i++)
            {
                Console.WriteLine($"{empresas[i]}: {ventasAnuales[i]}");
            }

            // Ordenamiento burbuja
            for (int i = 0; i < ventasAnuales.Length - 1; i++)
            {
                for (int j = 0; j < ventasAnuales.Length - i - 1; j++)
                {
                    if (ventasAnuales[j] > ventasAnuales[j + 1])
                    {
                        int temp = ventasAnuales[j];
                        ventasAnuales[j] = ventasAnuales[j + 1];
                        ventasAnuales[j + 1] = temp;
                    }
                }
            }

            // Mostrar ordenadas
            Console.WriteLine("\n Ventas ordenadas (menor a mayor):");
            for (int i = 0; i < ventasAnuales.Length; i++)
            {
                Console.WriteLine($"{i + 1}. {ventasAnuales[i]} juegos");
            }

            Console.ReadKey();
        }
        // Función para calcular el promedio de precios de los videojuegos
        static void calcularPromedioprecio()
        {
            double sumaPrecios = 0;
            int contador = 0;
            // Calcular la suma de precios y contar videojuegos disponibles
            foreach (var juego in videoJuegos)
            {
                if (juego.disponible)
                {
                    sumaPrecios += juego.precio;
                    contador++;
                }
            }
            // Mostrar el promedio si hay videojuegos disponibles
            if (contador > 0)
            {
                double promedio = sumaPrecios / contador;
                Console.WriteLine($"El precio promedio de los videojuegos disponibles es: {promedio}");
            }
            else
            {
                Console.WriteLine("No hay videojuegos disponibles para calcular el promedio.");
            }
        }
        // Función para filtrar videojuegos por edad mínima
        static void filtrarPoredadMinima()
        {
            Console.Write("Ingrese la edad máxima: ");
            int edadMaxima = int.Parse(Console.ReadLine());
            Console.WriteLine("--- Videojuegos Filtrados ---");
            // Usar un bucle foreach para filtrar y mostrar videojuegos
            foreach (var juego in videoJuegos)
            {
                if (juego.edadMinima <= edadMaxima)
                {
                    Console.WriteLine($"Nombre: {juego.nombre}, Edad Mínima: {juego.edadMinima}");
                }
            }
        }
            static void Main(string[] args)
            {
                videoJuegos.Add(new videoJuego
                {
                    nombre = "Aventuras Épicas",
                    categoria = 'A',
                    precio = 49.99,
                    edadMinima = 10,
                    disponible = true
                });
                videoJuegos.Add(new videoJuego
                {
                    nombre = "RoboWars",
                    categoria = 'R',
                    precio = 59.99,
                    edadMinima = 12,
                    disponible = false
                });
                videoJuegos.Add(new videoJuego
                {
                    nombre = "Puzzle Mania",
                    categoria = 'D',
                    precio = 19.99,
                    edadMinima = 5,
                    disponible = true
                });
                videoJuegos.Add(new videoJuego
                {
                    nombre = "Simulador de Vida",
                    categoria = 'A',
                    precio = 39.99,
                    edadMinima = 16,
                    disponible = true
                });
                videoJuegos.Add(new videoJuego
                {
                    nombre = "Carreras Extrema",
                    categoria = 'R',
                    precio = 29.99,
                    edadMinima = 7,
                    disponible = true
                });

                int opcion;
                do
                {
                    mostarMenu();
                    if (!int.TryParse(Console.ReadLine(), out opcion))
                    {
                        Console.WriteLine("Opción inválida. Ingrese un número del 1 al 8");
                        opcion = 0;
                    }
                    Console.Clear();

                    switch (opcion)
                    {
                        case 1:
                            agregarVideojuego();
                            break;
                        case 2:
                            buscarVideojuego();
                            break;
                        case 3:
                            listarVideojuegos();
                            break;
                        case 4:
                            ordenarVideojuegosPorprecio();
                            break;
                        case 5:
                            registrarVentas();
                            break;
                        case 6:
                            calcularPromedioprecio();
                            break;
                        case 7:
                            filtrarPoredadMinima();
                            break;
                        case 8:
                            Console.WriteLine("Saliendo del programa");
                            break;
                        default:
                            Console.WriteLine("Opción no válida. Intente nuevamente.");
                            break;

                    }

                } while (opcion != 8);

            }
        
    }
    
}

