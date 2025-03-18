# C-SISTEMA-DE-MONITORE-DE-AGUA
SISTEMA DE MONITOREO DEL CONSUMO DE AGUA EN EL HOGAR
#include <iostream>
#include <iomanip>
#include <vector>
#include <string>
#include <windows.h>

using namespace std;

const int MAX_MESES = 12;
const int MAX_CASAS = 100;
const double PRECIO_POR_LITRO = 0.005;

void setColor(int color)
{
    SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), color);
}

// ===============================================
//                CLASE USUARIO
// ===============================================

class Usuario
{
private:
    string nombre;
    string dni;
    string direccion;                                                               
    string descripcionConsumo;
    double consumo;
    double pagoMensual;

public:
    Usuario(string nombre, string dni, string direccion, string descripcionConsumo, double consumo)
        : nombre(nombre), dni(dni), direccion(direccion), descripcionConsumo(descripcionConsumo), consumo(consumo)
    {
        pagoMensual = consumo * PRECIO_POR_LITRO;
    }

    string getNombre() const { return nombre; }
    string getDNI() const { return dni; }
    double getConsumo() const { return consumo; }
    double getPagoMensual() const { return pagoMensual; }

    void recomendacionesAhorro() const
    {
        if (descripcionConsumo == "Alto" || descripcionConsumo == "alto")
        {
            cout << "\nConsejos para reducir el consumo de agua:\n"
                 << "- Reparar fugas en grifos y tuberías.\n"
                 << "- Usar regaderas de bajo consumo.\n"
                 << "- Lavar su auto con un balde en lugar de una manguera.\n"
                 << "- Recolectar agua de lluvia para riego.\n"
                 << "==============================\n";
        }
        if (descripcionConsumo == "Medio" || descripcionConsumo == "medio")
        {
            cout << "\nRecomendiones para mantener el consumo de agaua:\n"
                 << "- Vas por buen camino, sigue mnateniendo tu consumo de agua moderado!!.\n"
                 << "==============================\n";
        }
        if (descripcionConsumo == "Bajo" || descripcionConsumo == "bajo")
        {
            cout << "\nRecomendiones para mantener el consumo de agaua:\n"
                 << "- FELICIDADES, HACES UN BUEN MANEJO DE CONSUMO DE AGAU, SIGUE ASI!!.\n"
                 << "===================================================================\n";
        }
    }

    void mostrarBoleta() const
    {
        cout << "\n============================\n";
        cout << "    RECIBO DE PAGO DE AGUA" << endl;
        cout << "============================\n";
        cout << "Nombre: " << nombre << endl;
        cout << "DNI: " << dni << endl;
        cout << fixed << setprecision(2);
        cout << "Consumo mensual: " << consumo << " m³" << endl;
        cout << "Pago mensual estimado: S/" << pagoMensual << endl;
        cout << "============================\n";
    }
};

// ===============================================
//                CLASE FAMILIA
// ===============================================

class Familia
{
private:
    string titular;
    string direccion;
    vector<Usuario> miembros;

public:
    Familia(string titular, string direccion) : titular(titular), direccion(direccion) {}

    void agregarMiembro(const Usuario &usuario)
    {
        miembros.push_back(usuario); // guarda los datos de lo miembros registrados
    }

    const vector<Usuario> &getMiembros() const { return miembros; }
};

// ===============================================
//                CLASE SISTEMA
// ===============================================

class SistemaConsumo
{
private:
    vector<Familia> familias;
    double consumo[MAX_CASAS][MAX_MESES] = {{0}};

public:
    void registrarFamilia()
    {
        string titular, direccion, nombre, dni, descripcionConsumo;
        double consumo;
        int opcion;

        cout << "Ingrese el nombre del titular: ";
        getline(cin, titular);
        cout << "Dirección de casa: ";
        getline(cin, direccion);
        do
        {
            cout << "DNI: ";
            cin >> dni;
            if (dni.length() != 8)
            {
                cout << "Error: El DNI debe tener exactamente 8 dígitos.\n";
            }
        } while (dni.length() != 8);

        Familia nuevaFamilia(titular, direccion);
        cout << "\n============================\n";
        cout << "    Seleccione una opción:     ";
        cout << "\n============================\n";
        cout << "1. Vive solo\n";
        cout << "2. Vive con su familia\n";
        cout << "3. Alquila habitaciones";
        cout << "\n============================\n";
        cout << "Opción: ";
        cin >> opcion;
        cin.ignore();

        int numPersonas = (opcion == 1) ? 1 : 0;
        if (opcion == 1)
        {
            nombre = titular; // Asigna el nombre del titular
            cout << "Consumo de agua en metros cúbicos: ";
            cin >> consumo;
            cin.ignore();
            cout << "Descripción del consumo: ";
            getline(cin, descripcionConsumo);
            // Crear usuario y agregarlo a la familia
            Usuario usuario(nombre, dni, direccion, descripcionConsumo, consumo);
            nuevaFamilia.agregarMiembro(usuario);
            // usuario.recomendacionesAhorro();
            // Agregar la familia completa a la lista
            familias.push_back(nuevaFamilia);
            // usuario.recomendacionesAhorro();
            cout << "Registro completado de persona que vive sola.\n";
            cout << endl;
        }
        if (opcion == 2)
        {
            cout << "Ingrese el número de personas (incluyendo al titular): ";
            cin >> numPersonas;
            cin.ignore();
            for (int i = 0; i < numPersonas; i++)
            {
                cout << "\nIngrese los datos de la persona " << (i + 1) << ":\n";
                do
                {
                    cout << "Nombre: ";
                    cin >> nombre;
                    cout << "DNI: ";
                    cin >> dni;
                    if (dni.length() != 8)
                    {
                        cout << "Error: El DNI debe tener exactamente 8 dígitos.\n";
                    }
                } while (dni.length() != 8);
                // cout << "Consumo de agua en metros cúbicos: ";
                // cin >> consumo;
                // cin.ignore();
                // cout << "Descripción del consumo: ";
                // getline(cin, descripcionConsumo);
            }
            cout << "Consumo de agua en metros cúbicos: ";
            cin >> consumo;
            cin.ignore();
            cout << "Descripción del consumo: ";
            getline(cin, descripcionConsumo);
            Usuario usuario(nombre, dni, direccion, descripcionConsumo, consumo);
                nuevaFamilia.agregarMiembro(usuario);
                familias.push_back(nuevaFamilia);
                // usuario.recomendacionesAhorro();
        }
        if (opcion == 3)
        {
            cout << "Ingrese el número de personas : ";
            cin >> numPersonas;
            cin.ignore();
            for (int i = 0; i < numPersonas; i++)
            {
                cout << "\nIngrese los datos de la persona " << (i + 1) << ":\n";
                do
                {
                    cout << "Nombre: ";
                    cin >> nombre;
                    cout << "DNI: ";
                    cin >> dni;
                    if (dni.length() != 8)
                    {
                        cout << "Error: El DNI debe tener exactamente 8 dígitos.\n";
                    }
                } while (dni.length() != 8);
                cout << "Consumo de agua en metros cúbicos: ";
                cin >> consumo;
                cin.ignore();
                cout << "Descripción del consumo: ";
                getline(cin, descripcionConsumo);
                Usuario usuario(nombre, dni, direccion, descripcionConsumo, consumo);
                nuevaFamilia.agregarMiembro(usuario);
                familias.push_back(nuevaFamilia);
                // usuario.recomendacionesAhorro();
            }
        }
    }

    void registrarConsumo(int numCasas) {
        int casa, mes;
        double litros;
        string nombresMeses[MAX_MESES] = {"Enero", "Febrero", "Marzo", "Abril", "Mayo", "Junio",
                                              "Julio", "Agosto", "Septiembre", "Octubre", "Noviembre", "Diciembre"};
    
        cout << "Ingrese el número de la casa (1 - " << numCasas << "): ";
        cin >> casa;
        if (casa < 1 || casa > numCasas) {
            cout << "Número de casa no válido.\n";
            return;
        }
    
        cout << "Ingrese el número del mes (1 - 12): ";
        cin >> mes;
        if (mes < 1 || mes > MAX_MESES) {
            cout << "Número de mes no válido.\n";
            return;
        }
    
        cout << "Mes seleccionado: " << nombresMeses[mes - 1] << endl;
    
        cout << "Ingrese el consumo en litros: ";
        cin >> litros;
        if (litros < 0) {
            cout << "El consumo no puede ser negativo.\n";
            return;
        }
    
        consumo[casa - 1][mes - 1] = litros;
        cout << "Consumo registrado: " << litros << " litros para la casa " << casa << " en el mes de " << nombresMeses[mes - 1] << ".\n";
    }

    void mostrarHistorial() const
    {
        if (familias.empty())
        {
            cout << "No hay familias registradas.\n";
            return;
        }

        for (const auto &familia : familias)
        {
            for (const auto &miembro : familia.getMiembros())
            {
                miembro.mostrarBoleta();
                miembro.recomendacionesAhorro();
            }
        }
    }
};

    // ===============================================
    //               FUNCIÓN PRINCIPAL
    // ===============================================

    int main()
    {
        SistemaConsumo sistema;
        int opcion, numCasas;

        cout << "Ingrese la cantidad de casas a monitorear: ";
        cin >> numCasas;
        cin.ignore();
        cout << endl;

        if (numCasas <= 0 || numCasas > MAX_CASAS)
        {
            cout << "Número de casas no válido.\n";
            return 1;
        }

        do
        {
            setColor(11);
            cout << "  ⠀⣠⠞⣩⣭⣉⡙⠒⢤⡀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀" << endl;
            cout << "  ⢸⣇⣺⡏⠀⠀⠈⠳⡄⣝⢦⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀" << endl;
            cout << "   ⠻⠋⠀⠀⠀ ⠀⠀⢻⡹⣆⠳⣄⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀" << endl;
            cout << "   ⢠⡄⠀⠀⠀⠀⠀⠀⠀⣧⢹⢈⣷⡀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀" << endl;
            cout << "   ⠈⠁⠀⠀⠀⠀⠀⠀⠀⣿⠀⣿⠈⢿⣿⣦⡀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀" << endl;
            cout << "   ⣼⢻⡇⠀⠀⠀⠀⠀⠀⣿⠈⠈⣄⠑⡿⡿⣦⠀⠀⠀⠀⠀⠀⠀⠀⠀" << endl;
            cout << "   ⢼⡀⢹⡆⠀⠀⠀⠀⠀⡿⠀⣼⠀⡜⠈⢪⡫⣿⣄⠀⠀⠀⠀⠀⠀⠀" << endl;
            cout << "⠀⠀⠀       ⠀ ⢰⠇⠀⠀ ⡇⠹⠀⠀⠑⣜⢿⢷⣄⠀⠀⠀⠀⠀⠀⠀⠀" << endl;
            cout << "⠀⠀⠀⠀⠀  ⠀⠀⠀⢀⡿⠀⠀⠀⠀⠇⠀⢣⠀⠀⠈⠢⡱⡝⢷⡀⠀⠀⠀⠀⠀⠀⠀⠀" << endl;
            cout << "⠀⠀⠀⠀     ⠀⣼⠁⠀⣠⣶⣤⡀⠀⠈⡆⠀⣴⠶⠷⣮⢆⠹⣦⡀⠀⠀⠀⠀⠀⠀⠀" << endl;
            cout << "⠀⠀⠀⠀   ⠀⠀⣼⠃⡜⠘⠋⠁⠸⠀⠀⠀⠸⠀⠀⠀⣀⣈⣣⢣⡈⢳⣄⠀⠀⠀⠀⠀⠀" << endl;
            cout << "⠀⠀   ⠀⠀⠀⡼⠃⡜⢀⣤⡾⠿⢷⣦⡀⠀⠀⡆⣴⠟⠓⠚⠻⢷⣵⡀⠙⣧⠀⠀⠀⠀⠀" << endl;
            cout << "⠀   ⠀⠀⠀⡼⢃⠌⢀⡾⠉⣠⣄⡀⠈⠻⣄⠀⢸⢇⣾⣷⣦⡀⠀⢹⣷⡀⠈⢷⡀⠀⠀⠀" << endl;
            cout << "⠀   ⠀⢀⡼⢁⠎⠀⢸⠁⣼⠋⣿⣽⡀⠀⢿⠆⣿⢸⣇⣼⣿⡇⠀⠀⣧⠀⠀⠀⢻⡄⠀⠀" << endl;
            cout << "⠀   ⠀⡼⢁⠎⠀⠀⢻⠀⣿⣾⣿⣿⡇⠀⣼⡆⠸⡟⣿⣿⣿⡇⠀⢀⡟⠀⠀⠀⠀⢻⡀⠀" << endl;
            cout << "⠀   ⣸⠃⠆⠀⠀⠀⠘⣦⠘⠿⣿⠟⠀⣠⡟⡇⡇⠻⣮⣉⠉⣀⣠⡾⠣⠒⠒⠀⠀⠀⢿⠀" << endl;
            cout << "   ⢠⡇⠈⠀⠀⢀⣀⣀⠘⢷⣤⡤⣤⣾⠛⠀⠉⠀⠀⠈⠛⠛⠻⣿⣅⣀⣀⡀⠀⠀⠀⠈⣇  " << endl;
            cout << "   ⣸⠀⠀⢀⠆⢇⡀⠀⡹⠀⠉⡽⠉⠀⠀⠀⠀⠀⠀⠀⠀⠀⢠⢼⣾⠀⠀⠀⠀⠀⠀⠀   ⢿    " << endl;
            cout << "   ⡇⠀⠀⠘⠀⠀⠐⠲⡶⠟⣿⣦⣤⢀⣀⠀⠀⠀⢀⠠⣴⡮⣿⣿⣿⠀⠀⠀⠀⠀⠀⠀ ⢸   " << endl;
            cout << "   ⡇⠀⠀⠀⠀⠀⠀⠀⠹⠀⠘⣿⣿⣿⣶⣖⠒⠒⠚⢋⣉⣴⣿⣿⡏⠀⠀⠀⠀⠀⠀⠀ ⢸   " << endl;
            cout << "   ⡇⠄⠀⠸⡀⠀⠀⠀⠀⢇⠀⠘⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⠇⠀⠀⠀⠀⠀⠀⡀⣿   " << endl;
            cout << "   ⢹⡀⠀⠀⠡⡀⠀⠀⠀⢸⠀⠀⠈⢯⡋⢿⣻⣝⠟⠻⣿⣿⣿⡏⠀⠀⠀⠀⠀⠀⠀⢰⠇   " << endl;
            cout << "   ⠘⣧⠀⠀⠀⠙⠂⠀⠐⠁⡀⠀⠀⠙⣟⣆⡈⠛⠀⠀⠀⢹⣿⠆⠀⠀⠀⠀⠀⢠⢡⡟⠀   " << endl;
            cout << "⠀   ⠘⣧⠀⠀⠀⠀⠀⠀⢯⣬⡇⠀⠀⠳⢷⣽⣖⣩⣴⢞⡷⠃⠀⠀⠀⠀⠀⡰⣱⠟⠀⠀   " << endl;
            cout << "⠀   ⠀⠘⢷⡄⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠉⠙⠛⠛⠉⠀⠀⠀⠀⠀⠀⣠⡾⠋⠀⠀⠀    " << endl;
            cout << "⠀    ⠀⠠⠀⠙⢶⣄⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢀⣠⡾⠋⠀⠀⠀⠀⠀      " << endl;
            cout << "⠰⠊⠁⠀⠀⠀⠀⠀⠀⠉⠻⢶⣤⣀⡀⠀⠀⠀⠀⠀⠀⠀⠀⣀⣀⣤⠶⠛⠉⠈⠁⠒⢄⡀⠀⠀" << endl;
            cout << "⠘⠢⢄⡀⠀⠀⠀⠀⠀⠀⠀⠀⠈⠉⠛⠛⠓⠶⠶⠶⠒⠛⠛⠋⠉⠀⠀⠀⠀⠀⠀⠀⠀⡹⠀⠀" << endl;
            cout << "⠀⠀⠀⠀⠉⠒⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠉⠀⠀⠀    " << endl;
            setColor(1);
            cout << "==========================================\n";
            cout << "|      SISTEMA DE CONSUMO DE AGUA        |\n";
            cout << "==========================================\n";
            cout << "| 1. Registrar casa                      |\n";
            cout << "| 2. Registrar consumo mensual por casa  |\n";
            cout << "| 3. Mostrar historial de consumo        |\n";
            cout << "| 4. Salir                               |\n";
            cout << "==========================================\n";
            setColor(2);
            cout << "Seleccione una opción: ";
            cin >> opcion;
            cin.ignore();

            switch (opcion)
            {
            case 1:
                sistema.registrarFamilia();
                break;
            case 2:
                sistema.registrarConsumo(numCasas);
                break;  
            case 3:
                sistema.mostrarHistorial();
                break;
            case 4:
                cout << "Saliendo del sistema...\n";
                break;
            default:
                cout << "Opción no válida, intenta de nuevo.\n";
            }
        } while (opcion != 4);

        return 0;
    }
