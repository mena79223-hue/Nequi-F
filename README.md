# Nequi-F
Imitación nequi
// Archivo: mockData.js (para usar en React Native o similar)

export const USER_PROFILE = {
    nombre: "Usuario Demo",
    celular: "300 123 4567",
    saldoDisponible: 100000.00, // Saldo inicial fijo para el entrenamiento
    cuenta: "Ahorros Nequi",
    documento: "CC 1.000.000.000"
};

export const MOVIMIENTOS_INICIALES = [
    {
        id: "t001",
        tipo: "Envío Recibido",
        descripcion: "Transferencia de mamá/papá",
        valor: 50000.00,
        fecha: "05/Oct/2025"
    },
    {
        id: "t002",
        tipo: "Pago de Servicio",
        descripcion: "Pagar factura de luz",
        valor: -25000.00,
        fecha: "04/Oct/2025"
    },
    {
        id: "t003",
        tipo: "Recarga de celular",
        descripcion: "Recarga a 301 987 6543",
        valor: -10000.00,
        fecha: "04/Oct/2025"
    }
];

export const BOLSOS_SIMULADOS = [
    {
        nombre: "Colchón de Emergencia",
        valor: 50000.00,
        meta: 100000.00
    },
    {
        nombre: "Viaje a la Costa",
        valor: 20000.00,
        meta: 500000.00
    }
];

export const COLORES_NEQUI = {
    principal: '#FF0054', // Rojo/Rosado Nequi
    secundario: '#F5F5F5', // Gris Claro para fondos
    textoOscuro: '#333333',
    textoClaro: '#FFFFFF',
};
// Archivo: transactionSimulator.js (Lógica de control para la simulación)

let saldoActual = 100000.00; // Usa el valor inicial de mockData.js
let historial = [...MOVIMIENTOS_INICIALES]; // Crea una copia para modificar

// Función principal para SIMULAR ENVIAR dinero
export const simularEnvio = (monto, destino, tipo) => {
    const montoNumerico = parseFloat(monto);
    if (montoNumerico > 0 && montoNumerico <= saldoActual) {
        
        // 1. Actualiza el saldo simulado
        saldoActual -= montoNumerico;

        // 2. Crea un registro de movimiento
        const nuevoMovimiento = {
            id: `t${Date.now()}`,
            tipo: "Envío de Plata",
            descripcion: `Envío a ${destino} (${tipo})`,
            valor: -montoNumerico,
            fecha: new Date().toLocaleDateString('es-CO')
        };
        
        // 3. Agrega al historial simulado
        historial.unshift(nuevoMovimiento); 

        // 4. Retorna el nuevo estado de la app para la interfaz
        return { 
            success: true, 
            nuevoSaldo: saldoActual, 
            comprobante: nuevoMovimiento
        };

    } else {
        return { 
            success: false, 
            mensaje: "Error: Saldo insuficiente o monto inválido en la simulación." 
        };
    }
};

// Función para obtener el estado actual
export const obtenerEstadoActual = () => {
    return {
        saldo: saldoActual,
        movimientos: historial
    };
};

// NOTA: Para mover plata a "Bolsillos" necesitarías funciones similares que ajusten el saldo
// y el valor dentro de la estructura de BOLSOS_SIMULADOS.
