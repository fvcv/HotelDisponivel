using System;
using System.Collections.Generic;

// Classe que representa um hóspede
public class Pessoa
{
    public string Nome { get; set; }
    public string Sobrenome { get; set; }
    
    public Pessoa(string nome, string sobrenome)
    {
        Nome = nome;
        Sobrenome = sobrenome;
    }
    
    public string NomeCompleto => $"{Nome} {Sobrenome}";
}

// Classe que representa uma suíte do hotel
public class Suite
{
    public string TipoSuite { get; set; }
    public int Capacidade { get; set; }
    public decimal ValorDiaria { get; set; }
    
    public Suite(string tipoSuite, int capacidade, decimal valorDiaria)
    {
        TipoSuite = tipoSuite;
        Capacidade = capacidade;
        ValorDiaria = valorDiaria;
    }
}

// Classe que gerencia a reserva
public class Reserva
{
    public List<Pessoa> Hospedes { get; set; }
    public Suite Suite { get; set; }
    public int DiasReservados { get; set; }
    
    public Reserva(int diasReservados)
    {
        DiasReservados = diasReservados;
        Hospedes = new List<Pessoa>();
    }
    
    public void CadastrarHospedes(List<Pessoa> hospedes)
    {
        if (Suite == null)
        {
            throw new Exception("Suíte não foi definida para esta reserva.");
        }
        
        if (hospedes.Count > Suite.Capacidade)
        {
            throw new Exception($"Capacidade excedida. A suíte {Suite.TipoSuite} suporta até {Suite.Capacidade} hóspedes.");
        }
        
        Hospedes.AddRange(hospedes);
    }
    
    public void CadastrarSuite(Suite suite)
    {
        Suite = suite;
    }
    
    public int ObterQuantidadeHospedes()
    {
        return Hospedes.Count;
    }
    
    public decimal CalcularValorDiaria()
    {
        decimal valorTotal = DiasReservados * Suite.ValorDiaria;
        
        // Aplica desconto de 10% para reservas com mais de 10 dias
        if (DiasReservados > 10)
        {
            valorTotal *= 0.9m; // 10% de desconto
        }
        
        return valorTotal;
    }
}

// Programa principal para testar o sistema
class Program
{
    static void Main(string[] args)
    {
        // Criando suítes disponíveis
        Suite suitePremium = new Suite("Premium", 4, 250.00m);
        Suite suiteLuxo = new Suite("Luxo", 2, 150.00m);
        
        // Criando hóspedes
        Pessoa hospede1 = new Pessoa("João", "Silva");
        Pessoa hospede2 = new Pessoa("Maria", "Santos");
        Pessoa hospede3 = new Pessoa("Pedro", "Oliveira");
        
        // Criando uma reserva para 12 dias
        Reserva reserva = new Reserva(12);
        
        try
        {
            // Cadastrando a suíte e os hóspedes
            reserva.CadastrarSuite(suitePremium);
            reserva.CadastrarHospedes(new List<Pessoa> { hospede1, hospede2, hospede3 });
            
            // Exibindo informações da reserva
            Console.WriteLine("Reserva realizada com sucesso!");
            Console.WriteLine($"Hóspedes: {reserva.ObterQuantidadeHospedes()}");
            Console.WriteLine($"Suíte: {reserva.Suite.TipoSuite}");
            Console.WriteLine($"Valor total da estadia: R$ {reserva.CalcularValorDiaria():F2} (com desconto de 10% para mais de 10 dias)");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Erro ao realizar reserva: {ex.Message}");
        }
    }
}
