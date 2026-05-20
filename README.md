#include <iostream>
#include <string>
#include <iomanip>
using namespace std;

struct Producto {
    string nombre;
    int    codigo;
    int    cantidad;
    double precio;
};


//  Estado del inventario

const int CAPACIDAD_MAX = 100;
Producto inventario[CAPACIDAD_MAX];
int totalProductos = 0;


int buscarProducto(int codigoBusqueda) {
    for (int i = 0; i < totalProductos; i++) {
        if (inventario[i].codigo == codigoBusqueda) {
            return i;   // índice encontrado
        }
    }
    return -1;          // no encontrado
}

int mostrarMenu() {
    int opcion;
    cout << "\n╔══════════════════════════════╗" << endl;
    cout << "║   GADGETXPRESS - INVENTARIO  ║" << endl;
    cout << "╠══════════════════════════════╣" << endl;
    cout << "║  1. Agregar producto         ║" << endl;
    cout << "║  2. Listar productos         ║" << endl;
    cout << "║  3. Actualizar cantidad      ║" << endl;
    cout << "║  4. Reporte bajo stock       ║" << endl;
    cout << "║  5. Salir                    ║" << endl;
    cout << "╚══════════════════════════════╝" << endl;
    cout << "Seleccione una opcion: ";
    cin  >> opcion;
    return opcion;
}

void agregarProducto() {
    cout << "\n>>> AGREGAR PRODUCTO <<<" << endl;

    if (totalProductos >= CAPACIDAD_MAX) {
        cout << "Error: El inventario esta lleno (maximo " << CAPACIDAD_MAX << " productos)." << endl;
        return;
    }

    Producto nuevo;

    cout << "Nombre del producto : ";
    cin.ignore();
    getline(cin, nuevo.nombre);

    cout << "Codigo unico        : ";
    cin  >> nuevo.codigo;

    // Validar duplicado usando la función de ayuda
    if (buscarProducto(nuevo.codigo) != -1) {
        cout << "Error: Ya existe un producto con el codigo " << nuevo.codigo << "." << endl;
        return;
    }

    cout << "Cantidad inicial    : ";
    cin  >> nuevo.cantidad;

    cout << "Precio unitario     : ";
    cin  >> nuevo.precio;

    inventario[totalProductos] = nuevo;
    totalProductos++;

    cout << "Producto '" << nuevo.nombre << "' agregado correctamente." << endl;
}

void listarProductos() {
    cout << "\n>>> LISTADO DE PRODUCTOS <<<" << endl;

    if (totalProductos == 0) {
        cout << "El inventario esta vacio." << endl;
        return;
    }

    cout << left
         << setw(6)  << "COD"
         << setw(20) << "NOMBRE"
         << setw(10) << "CANTIDAD"
         << setw(12) << "PRECIO"
         << endl;
    cout << string(48, '-') << endl;

    for (int i = 0; i < totalProductos; i++) {
        cout << left
             << setw(6)  << inventario[i].codigo
             << setw(20) << inventario[i].nombre
             << setw(10) << inventario[i].cantidad
             << "$" << fixed << setprecision(2) << inventario[i].precio
             << endl;
    }
}

void actualizarCantidad() {
    cout << "\n>>> ACTUALIZAR CANTIDAD <<<" << endl;

    int codigoBusqueda;
    cout << "Ingrese el codigo del producto: ";
    cin  >> codigoBusqueda;

    int indice = buscarProducto(codigoBusqueda);  // reutiliza la función de ayuda

    if (indice == -1) {
        cout << "Error: No se encontro ningun producto con el codigo "
             << codigoBusqueda << "." << endl;
        return;
    }

    cout << "Producto  : " << inventario[indice].nombre << endl;
    cout << "Cantidad actual: " << inventario[indice].cantidad << endl;
    cout << "Nueva cantidad : ";

    int nuevaCantidad;
    cin  >> nuevaCantidad;

    inventario[indice].cantidad = nuevaCantidad;
    cout << "Cantidad actualizada correctamente." << endl;
}

void generarReporte() {
    const int UMBRAL_BAJO_STOCK = 5;
    cout << "\n>>> REPORTE DE BAJO INVENTARIO (menos de "
         << UMBRAL_BAJO_STOCK << " unidades) <<<" << endl;

    bool hayBajoStock = false;

    for (int i = 0; i < totalProductos; i++) {
        if (inventario[i].cantidad < UMBRAL_BAJO_STOCK) {
            cout << "  [!] " << inventario[i].nombre
                 << " | Cod: " << inventario[i].codigo
                 << " | Stock: "  << inventario[i].cantidad
                 << " unidades" << endl;
            hayBajoStock = true;
        }
    }

    if (!hayBajoStock) {
        cout << "Todos los productos tienen stock suficiente." << endl;
    }
}

void cargarDatosDePrueba() {
    inventario[0] = {"Tornillo Hex", 10, 15, 500.0};
    inventario[1] = {"Tuerca 1/2",   20,  2, 200.0};
    inventario[2] = {"Arandela",     30, 20, 100.0};
    totalProductos = 3;
}


int main() {
    cargarDatosDePrueba();

    int opcion;
    do {
        opcion = mostrarMenu();

        switch (opcion) {
            case 1: agregarProducto();   break;
            case 2: listarProductos();   break;
            case 3: actualizarCantidad(); break;
            case 4: generarReporte();    break;
            case 5: cout << "Saliendo del sistema. ¡Hasta luego!" << endl; break;
            default: cout << "Opcion invalida. Intente de nuevo." << endl;
        }
    } while (opcion != 5);

    return 0;
}
