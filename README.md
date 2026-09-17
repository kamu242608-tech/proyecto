main.java
import java.util.ArrayList;
import java.util.List;

// ==================== CLASE ABSTRACTA ====================
abstract class Vehiculo {
    // Encapsulamiento: atributos privados e inmutables
    private final String marca;
    private final String modelo;
    private final double tarifaBase;

    public Vehiculo(String marca, String modelo, double tarifaBase) {
        this.marca = marca;
        this.modelo = modelo;
        this.tarifaBase = tarifaBase;
    }

    // Solo getters (no setters)
    public String getMarca() {
        return marca;
    }

    public String getModelo() {
        return modelo;
    }

    public double getTarifaBase() {
        return tarifaBase;
    }

    // Método abstracto → Abstracción + permite Polimorfismo
    public abstract double calcularCostoAlquiler(int dias);

    @Override
    public String toString() {
        return String.format("%s [marca=%s, modelo=%s, tarifaBase=$%.1f/día]",
                getClass().getSimpleName(), marca, modelo, tarifaBase);
    }
}

// ==================== AUTO ====================
class Auto extends Vehiculo {
    private static final double RECARGO_SEGURO_POR_DIA = 10.0;

    public Auto(String marca, String modelo, double tarifaBase) {
        super(marca, modelo, tarifaBase);
    }

    @Override
    public double calcularCostoAlquiler(int dias) {
        // (tarifaBase + 10) * días
        return (getTarifaBase() + RECARGO_SEGURO_POR_DIA) * dias;
    }
}

// ==================== MOTO ====================
class Moto extends Vehiculo {
    private static final double RECARGO_CASCO = 5.0;

    public Moto(String marca, String modelo, double tarifaBase) {
        super(marca, modelo, tarifaBase);
    }

    @Override
    public double calcularCostoAlquiler(int dias) {
        // (tarifaBase * días) + 5
        return (getTarifaBase() * dias) + RECARGO_CASCO;
    }
}

// ==================== MAIN ====================
public class Main {
    public static void main(String[] args) {
        List<Vehiculo> vehiculos = new ArrayList<>();

        // Al menos un Auto y una Moto
        vehiculos.add(new Auto("Toyota", "Corolla", 45.0));
        vehiculos.add(new Moto("Honda", "CBR500", 30.0));
        vehiculos.add(new Auto("Ford", "Focus", 50.0));
        vehiculos.add(new Moto("Yamaha", "MT-07", 35.0));

        int dias = 5;

        System.out.println("=== Sistema de Alquiler de Vehículos ===");
        System.out.println("Cálculo de costo para " + dias + " días:\n");

        // Recorrido polimórfico
        for (Vehiculo v : vehiculos) {
            double costo = v.calcularCostoAlquiler(dias);
            System.out.println(v);
            System.out.printf("  → Costo de alquiler: $%.2f%n%n", costo);
        }
    }
}
