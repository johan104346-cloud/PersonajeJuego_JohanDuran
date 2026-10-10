using System;

interface ICurable
{
    void Curar();
}

interface IDefendible
{
    void Defender();
}

abstract class Personaje
{
    public string Nombre { get; set; }
    public int Vida { get; private set; }
    public int Nivel { get; private set; }

    public Personaje(string nombre, int vida, int nivel)
    {
        Nombre = nombre;
        Vida = Math.Max(1, Math.Min(100, vida));
        Nivel = Math.Max(1, nivel);
    }

    public void MostrarEstado()
    {
        Console.WriteLine("Nombre: " + Nombre);
        Console.WriteLine("Vida: " + Vida);
        Console.WriteLine("Nivel: " + Nivel);
    }

    public abstract void Atacar();

    public void RecibirDanio(int cantidad)
    {
        if (cantidad <= 0)
        {
            Console.WriteLine("el daño debe ser mayor que cero.");
            return;
        }

        Vida = Math.Max(0, Vida - cantidad);

        Console.WriteLine(Nombre + " recibe " + cantidad + " puntos de daño.");
        Console.WriteLine("vida restante: " + Vida);

        if (Vida == 0)
        {
            Console.WriteLine(Nombre + " ha sido derrotado.");
        }
    }

    protected void RecuperarVida(int cantidad)
    {
        int vidaAnterior = Vida;
        Vida = Math.Min(100, Vida + cantidad);

        Console.WriteLine("vida recuperada: " + (Vida - vidaAnterior));
        Console.WriteLine("vida actual: " + Vida);
    }
}

class Guerrero : Personaje, IDefendible
{
    public int Fuerza { get; set; }

    public Guerrero(string nombre, int vida, int nivel, int fuerza)
        : base(nombre, vida, nivel)
    {
        Fuerza = fuerza;
    }

    public override void Atacar()
    {
        Console.WriteLine(Nombre + " ataca con su espada causando 30 puntos de daño.");
    }

    public void Defender()
    {
        Console.WriteLine(Nombre + " levanta su escudo y se prepara para defenderse.");
    }
}

class Mago : Personaje, ICurable
{
    public int Mana { get; set; }

    public Mago(string nombre, int vida, int nivel, int mana)
        : base(nombre, vida, nivel)
    {
        Mana = mana;
    }

    public override void Atacar()
    {
        Console.WriteLine(Nombre + " lanza un hechizo causando 40 puntos de daño.");
    }

    public void Curar()
    {
        Console.WriteLine(Nombre + " utiliza magia de curacion.");
        RecuperarVida(20);
    }
}

class Arquero : Personaje
{
    public int Flechas { get; private set; }
    private int contadorAtaques = 0;

    public Arquero(string nombre, int vida, int nivel, int flechas) : base(nombre, vida, nivel)
    {
        Flechas = Math.Max(0, flechas);
    }

    public override void Atacar()
    {
        if (Flechas > 0)
        {
            int[] danos = { 15, 25, 35 };
            int dano = danos[contadorAtaques % danos.Length];

            Console.WriteLine(Nombre + " dispara una flecha causando " + dano + " puntos de daño.");
            Flechas--;
            contadorAtaques++;

            Console.WriteLine("Flechas restantes: " + Flechas);
        }
        else
        {
            Console.WriteLine(Nombre + " no tiene flechas para atacar.");
        }
    }
}

class Program
{
    static void Main(string[] args)
    {
        Guerrero guerrero = new Guerrero("Ares", 100, 5, 30);
        Mago mago = new Mago("Gandalf", 80, 6, 50);
        Arquero arquero = new Arquero("Robin", 90, 4, 10);

        Personaje[] personajes = { guerrero, mago, arquero };

        Console.WriteLine("==== PERSONAJES ====\n");

        foreach (Personaje personaje in personajes)
        {
            personaje.MostrarEstado();
            personaje.Atacar();
            Console.WriteLine("--------------------\n");
        }

        Console.WriteLine("===== HABILIDADES =====\n");

        guerrero.Defender();

        Console.WriteLine();
        mago.RecibirDanio(30);

        Console.WriteLine();
        mago.Curar();

        Console.WriteLine("\n===== ATAQUES DEL ARQUERO =====\n");

        arquero.Atacar();
        arquero.Atacar();
        arquero.Atacar();
        arquero.Atacar();

        Console.WriteLine("\n===== FIN DEL PROGRAMA =====");
    }
}
