# Hoteis

**Definição da classe "Reserva"**:

public class Reserva {

    private String nomeHotel;
    private int numeroQuarto;
    private String dataCheckIn;
    private String dataCheckOut;

    public Reserva(String nomeHotel, int numeroQuarto, String dataCheckIn, String dataCheckOut) {

        this.nomeHotel = nomeHotel;
        
        this.numeroQuarto = numeroQuarto;
        
        this.dataCheckIn = dataCheckIn;
        
        this.dataCheckOut = dataCheckOut;
    }

    public String getNomeHotel() {
    
        return nomeHotel;
    }

    public int getNumeroQuarto() {
    
        return numeroQuarto;
        
    }

    public String getDataCheckIn() {
    
        return dataCheckIn;
        
    }

    public String getDataCheckOut() {
    
        return dataCheckOut;
    }

    @Override
    
    public String toString() {
    
        return "Reserva{" +
        
                "nomeHotel='" + nomeHotel + '\'' +
                
                ", numeroQuarto=" + numeroQuarto +
                
                ", dataCheckIn='" + dataCheckIn + '\'' +
                
                ", dataCheckOut='" + dataCheckOut + '\'' +
                
                '}';
    }
}


**Função Hash e Tratamento de Colisões**: Para a função hash, é preciso criar uma função simples que distribua bem os números de identificação das reservas.

**Implementação do HashMap de Reservas**:

import java.util.LinkedList;

public class SistemaDeReservas {

    private LinkedList<Reserva>[] tabela;
    
    private int capacidade;

    public SistemaDeReservas(int capacidade) {
    
        this.capacidade = capacidade;
        
        tabela = new LinkedList[capacidade];
        
        for (int i = 0; i < capacidade; i++) {
        
            tabela[i] = new LinkedList<>();
        }
    }

    private int hashFunction(int chave) {
    
        return chave % capacidade;
    }

    public void inserirReserva(int idReserva, Reserva reserva) {
    
        int indice = hashFunction(idReserva);
        
        tabela[indice].add(reserva);
    }

    public Reserva buscarReserva(int idReserva) {
    
        int indice = hashFunction(idReserva);
        
        for (Reserva reserva : tabela[indice]) {
           
            if (reserva.hashCode() == idReserva) { // Supondo que hashCode é o idReserva
            
                return reserva;
            }
        }
        return null;
    }

    public boolean removerReserva(int idReserva) {
    
        int indice = hashFunction(idReserva);
        
        for (Reserva reserva : tabela[indice]) {
        
            if (reserva.hashCode() == idReserva) {
            
                tabela[indice].remove(reserva);
                
                return true;
            }
        }
        return false;
    }

    public static void main(String[] args) {
    
        SistemaDeReservas sistema = new SistemaDeReservas(10);

        Reserva reserva1 = new Reserva("Hotel A", 101, "2024-06-10", "2024-06-15");
        
        Reserva reserva2 = new Reserva("Hotel B", 102, "2024-06-11", "2024-06-16");

        sistema.inserirReserva(1, reserva1);
        
        sistema.inserirReserva(2, reserva2);
      
        System.out.println("Reserva 1: " + sistema.buscarReserva(1));
        
        System.out.println("Reserva 2: " + sistema.buscarReserva(2));

        sistema.removerReserva(1);
        
        System.out.println("Reserva 1 após remoção: " + sistema.buscarReserva(1));
    }
}
