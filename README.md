#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<stdbool.h>

#define MAX_WORKERS 10
#define MAX_CLIENTS 10
#define MAX_SUPPLIERS 10
#define MAX_PRODUCT 1000
#define MAX_ITEMS_FACTURA 1000
#define MAX_FACTURAS 1000
     
     
//STRUCTS
typedef struct{
    char nombre[60];
    int cedula;
    char tlf[20];
    char direc[60];
} Trabajador;

typedef struct{
    char nombre[60];
    int cedula;
    char tlf[20];
    char direc[60];
} Cliente;

typedef struct{
    char nombre[60];
    int cedula;
    char tlf[20];
    char direc[60];
} Proveedor;

typedef struct{
	int dia;
	int mes;
	int anio;
}Fecha;

typedef struct{
    char codigo[7];
	char nom[60];
	char desc[100];
	Fecha fecha_adquisicion; //DD-MM-AAAA
	int cantidad, ubi, categoria, tipo;
    float costo, precio_venta;
} Produc;

typedef struct {
    char codigo_producto[7];
    int cantidad;
    float precio_unitario; // en Bs
    float precio_dolares;
    float precio_euros;
    float precio_pesos;
    float subtotal;
    float subtotal_dolares;
    float subtotal_euros;
    float subtotal_pesos;
} ItemFactura;

//----------------------------------------
typedef struct {
    int numero_factura;
    Fecha fecha;
    int cedula_cliente;
    int cedula_trabajador;
    ItemFactura items[MAX_ITEMS_FACTURA];
    int num_items;
    float subtotal;
    float subtotal_dolares;
    float subtotal_euros;
    float subtotal_pesos;
    float iva;
    float iva_dolares;
    float iva_euros;
    float iva_pesos;
    float total;
    float total_dolares;
    float total_euros;
    float total_pesos;
} Factura;
//---------------------------------------------

Produc producto[MAX_PRODUCT];
Trabajador trab[MAX_WORKERS];
Cliente clien[MAX_CLIENTS];
Proveedor prov[MAX_SUPPLIERS];
int num_productos = 0;
int num_clientes = 0;
int num_trabajadores = 0;
int num_facturas = 0;
int num_proveedores = 0;

float tasa_dolar = 108.98;
float tasa_euro = 129.20;
float tasa_peso = 36.87;

void limpiar_buffer(); 
int leer_entero_positivo(const char *mensaje);
int leer_entero_no_negativo(const char *mensaje);
float leer_flotante_positivo(const char *mensaje);
bool validar_fecha(int dia, int mes, int anio);
void mostrarMenuPrincipal();
void mostrarSubMenuReportes();
void menu_productos();
void menu_clientes();
void menu_trabajadores();
void menu_facturas();
void leer_producto(int cantidad_a_agregar);
void mostrar_producto();
void leer_trabajador(int cantidad_a_agregar);
void mostrar_trabajador();
void leer_cliente(int cantidad_a_agregar);
void mostrar_cliente();
void leer_proveedor(int cantidad_a_agregar);
void mostrar_proveedor();
void modificar_trabajador();
int buscar_trabajador_por_cedula(int cedula_a_buscar);
void eliminar_trabajador();
void modificar_cliente();
int buscar_cliente_por_cedula(int cedula_a_buscar);
void eliminar_cliente();
void modificar_producto();
void eliminar_producto();
int buscar_producto_por_codigo(const char *codigo_a_buscar);
void crear_factura();
void mostrar_factura(int numero_factura);
void mostrar_ultimas_facturas();
void actualizar_tasas_cambio();
void mostrarSubMenuReportes();

int main() {
    int opcion_principal;
    int opcion_reportes;
    int opcion;
    int cont;
    do {
        mostrarMenuPrincipal();
        printf("\nSeleccione una opcion: ");
        scanf("%d", &opcion_principal);
        limpiar_buffer();
        system("cls");
        
        switch (opcion_principal) {
            case 1:
                printf("\nHa seleccionado Inventario.\n");
                
                //funcion inventario 
                break;
            case 2:
                printf("\nHa seleccionado Productos.\n");
                
            do{
                menu_productos();
                printf("Seleccione una opcion: ");
                scanf("%d", &opcion);
                limpiar_buffer();
        
        	switch (opcion) {
            	case 1: 
				if (num_productos >= MAX_PRODUCT) {
        		printf("El sistema ya esta lleno, no se pueden cargar mas productos\n");
        		} else{
        		printf("�Cuantos productos desea cargar al sistema?: ");
        		scanf("%d", &cont);
        		limpiar_buffer();    			
				leer_producto(cont);
				}
				break;
            	case 2: modificar_producto(); break;
            	case 3: eliminar_producto(); break;
            	case 4: mostrar_producto(); break;
            	case 0: break;
            	default: printf("Opcion invalida.\n");
            }
        	}while (opcion != 0);
                break;
            case 3:
                printf("\nHa seleccionado Clientes.\n");
            do{
                menu_clientes();
                printf("Seleccione una opcion: ");
        		scanf("%d", &opcion);
        		limpiar_buffer();
        	
        	switch (opcion) {
            	case 1: 
				if (num_productos >= MAX_CLIENTS) {
        		printf("El sistema ya esta lleno, no se pueden cargar mas clientes\n");
        		} else{
        		printf("�Cuantos clientes desea cargar al sistema?: ");
        		scanf("%d", &cont);
        		limpiar_buffer();    			
				leer_cliente(cont);
				}
				break;
            	case 2: modificar_cliente(); break;
            	case 3: eliminar_cliente(); break;
            	case 4: mostrar_cliente(); break;
           	 	case 0: break;
            	default: printf("Opcion invalida.\n");
        		}
    		} while (opcion != 0);
    		
                break;
            case 4:
                printf("\nHa seleccionado Facturas.\n");
            do{
                menu_facturas();
                printf("Seleccione una opcion: ");
        	scanf("%d", &opcion);
        	limpiar_buffer();
        
        	switch (opcion) {
            	case 1: /*crear_factura();*/ break;
            	case 2: /*mostrar_facturas();*/ break;
           		case 0: break;
            	default: printf("Opcion invalida.\n");
        		}
    		} while (opcion != 0);
                //funcion facturas
                break;
            case 5:
                printf("\nHa seleccionado Trabajadores.\n");
            do{
                menu_trabajadores();
                printf("Seleccione una opcion: ");
                scanf("%d", &opcion);
                limpiar_buffer();
        
        	switch (opcion) {
            	case 1: 
				if (num_trabajadores >= MAX_WORKERS) {
        		printf("El sistema ya esta lleno, no se pueden cargar mas trabajadores\n");
        		} else{
        		printf("�Cuantos trabajadores desea cargar al sistema?: ");
        		scanf("%d", &cont);
        		limpiar_buffer();    			
				leer_trabajador(cont);
				}
				break;
            	case 2: modificar_trabajador(); break;
            	case 3: eliminar_trabajador(); break;
            	case 4: mostrar_trabajador(); break;
            	case 0: break;
            	default: printf("Opcion invalida.\n");
        		}
    		} while (opcion != 0);
                //funcion trabajadores
                break;
                
            case 6: // Opcion para Reportes!
                do {
                    mostrarSubMenuReportes();
                    printf("Seleccione una opcion de reporte: ");
                    scanf("%d", &opcion_reportes);
                    limpiar_buffer();

                switch (opcion_reportes) {
                        // Opciones de INVENTARIO
                    case 1: printf("Inventario completo.\n"); break;
                    case 2: printf("Stock minimo.\n"); break;
                    case 3: printf("Productos danados.\n"); break;
                    
                        // Opciones de VENTAS/PRODUCTOS
                    case 4: printf("Valor y costo.\n"); break;
                    case 5: mostrar_tipos_zapatos_vendidos(); break;
                    case 6: printf("Total ventas por tipo de zapato.\n"); break;
                    case 7: mostrar_productos_mas_vendidos(); break;
                    case 8: mostrar_ultimas_facturas(); break;
                    
                        // Opciones de CLIENTES/TRABAJADORES
                    case 9: mostrar_clientes_con_compras(); break;
                    case 10: printf("Cantidad de trabajadores.\n"); break;
                    case 11: printf("Proveedores.\n"); break;
                    
                        // Opciones "Reportes" y "Salir" del submenu
                    case 12: printf("Volviendo al menu principal...\n"); break;
                    default: printf("Opcion de reporte no valida. Intente de nuevo.\n"); break;
                    }
        
                if (opcion_reportes != 12) { 
                    printf("Presione Enter para continuar...");
                    while (getchar() != '\n'); 
                    getchar();
                    }
                    } while (opcion_reportes != 12);
                break;
                
            case 7:
                printf("Saliendo del programa. �Hasta pronto!\n");
                break;
                
            default:
                printf("Opcion no valida. Por favor, intente de nuevo.\n");
                break;
        }
        
        if (opcion_principal != 7 && opcion_principal != 6) { 
            printf("Presione Enter para continuar...");
            while (getchar() != '\n'); 
            
            getchar(); 
        }
    
	} while (opcion_principal != 7); 

    return 0;
	}

//FUNCIONES MENU Y SUBMENU
void mostrarMenuPrincipal() {
    printf("\n============ MENU PRINCIPAL ============\n");
    printf("1. Inventario\n");
    printf("2. Productos\n");
    printf("3. Clientes\n");
    printf("4. Facturas\n");
    printf("5. Trabajadores\n");
    printf("6. Reportes\n");
    printf("7. Salir\n");
    printf("========================================\n");
}

void mostrarSubMenuReportes() {
    printf("\n======== SUBMENU: REPORTES ========\n");
    printf("\n--- INVENTARIO ---\n");
    printf("1. Inventario completo\n");
    printf("2. Stock minimo\n");
    printf("3. Productos danados\n");
    printf("\n--- VENTAS/PRODUCTOS ---\n");
    printf("4. Valor y costo\n");
    printf("5. Tipos de zapato mas y menos vendidos\n");
    printf("6. Total ventas por tipo de zapato\n");
    printf("7. Ultimas 7 facturas\n");
    printf("\n--- CLIENTES/TRABAJADORES ---\n");
    printf("8. Clientes que realizaron compras\n");
    printf("9. Cantidad de trabajadores\n");
    printf("10. Proveedores\n");
    printf("\n");
    printf("11. Volver al menu principal...\n");
    printf("========================================\n");
}

void menu_productos() {
        printf("\n--- MENU PRODUCTOS ---\n");
        printf("1. Agregar producto\n");
        printf("2. Modificar producto\n");
        printf("3. Eliminar producto\n");
        printf("4. Mostrar productos\n");
        printf("0. Volver al menu principal\n");
}

void menu_clientes() {
        printf("\n======== MENU CLIENTES ========\n");
        printf("1. Agregar cliente\n");
        printf("2. Modificar cliente\n");
        printf("3. Eliminar cliente\n");
        printf("4. Mostrar clientes\n");
        printf("0. Volver al menu principal\n");
        printf("====================================\n");
    }

void menu_trabajadores() {
        printf("\n======== MENU TRABAJADORES ========\n");
        printf("1. Agregar trabajador\n");
        printf("2. Modificar trabajador\n");
        printf("3. Eliminar trabajador\n");
        printf("4. Mostrar trabajadores\n");
        printf("0. Volver al menu principal\n");
        printf("====================================\n");
    }
 	
 	
 	void menu_facturas() {
    printf("\n======== MENU FACTURAS ========\n");
    printf("1. Crear factura\n");
    printf("2. Ver facturas\n");
    printf("0. Volver al menu principal\n");
    printf("====================================\n");
	}
    
    void mostrarSubMenuReportes() {
    printf("\n======== SUBMENU: REPORTES ========\n");
    printf("\n--- INVENTARIO ---\n");
    printf("1. Inventario completo\n");
    printf("2. Stock minimo\n");
    printf("3. Productos danados\n");
    printf("\n--- VENTAS/PRODUCTOS ---\n");
    printf("4. Valor y costo\n");
    printf("5. Tipos de zapato mas y menos vendidos\n");
    printf("6. Total ventas por tipo de zapato\n");
    printf("7. Productos mas vendidos\n");
    printf("8. Ultimas 7 facturas\n");
    printf("\n--- CLIENTES/TRABAJADORES ---\n");
    printf("9. Clientes que realizaron compras\n");
    printf("10. Cantidad de trabajadores\n");
    printf("11. Proveedores\n");
    printf("\n");
    printf("12. Volver al menu principal...\n");
    printf("========================================\n");
}

//-----------------FACTURAS----------------------
void crear_factura() {

    Factura facturas[MAX_FACTURAS];    int num_facturas = 0;
    if (num_facturas >= MAX_FACTURAS) {
        printf("No se pueden crear más facturas. Límite alcanzado.\n");
        return;
    }

    Factura nueva_factura;
    nueva_factura.numero_factura = num_facturas + 1;
    
    // fecha actual
    printf("Ingrese la fecha de la factura:\n");
    do {
        printf("Dia (1-31): ");
        scanf("%d", &nueva_factura.fecha.dia);
        limpiar_buffer();
        printf("Mes (1-12): ");
        scanf("%d", &nueva_factura.fecha.mes);
        limpiar_buffer();
        printf("Anio: ");
        scanf("%d", &nueva_factura.fecha.anio);
        limpiar_buffer();

        if (!validar_fecha(nueva_factura.fecha.dia, nueva_factura.fecha.mes, nueva_factura.fecha.anio)) {
            printf("Fecha invalida. Por favor, ingrese una fecha valida.\n");
        }
    } while (!validar_fecha(nueva_factura.fecha.dia, nueva_factura.fecha.mes, nueva_factura.fecha.anio));

    // Obtener cliente 
    mostrar_cliente();
    nueva_factura.cedula_cliente = leer_entero_positivo("Ingrese la cedula del cliente: ");
    int indice_cliente = buscar_cliente_por_cedula(nueva_factura.cedula_cliente);
    if (indice_cliente == -1) {
        printf("Cliente no encontrado. Factura no creada.\n");
        return;
    }

    // Obtener trabajador
    mostrar_trabajador();
    nueva_factura.cedula_trabajador = leer_entero_positivo("Ingrese la cedula del trabajador (vendedor): ");
    int indice_trabajador = buscar_trabajador_por_cedula(nueva_factura.cedula_trabajador);
    if (indice_trabajador == -1) {
        printf("Trabajador no encontrado. Factura no creada.\n");
        return;
    }

    // Agregar productos a la factura
    nueva_factura.num_items = 0;
    nueva_factura.subtotal = 0;
    nueva_factura.subtotal_dolares = 0;
    nueva_factura.subtotal_euros = 0;
    nueva_factura.subtotal_pesos = 0;
    char continuar = 'S';

    while (continuar == 'S' || continuar == 's') {
        if (nueva_factura.num_items >= MAX_ITEMS_FACTURA) {
            printf("No se pueden agregar más items a esta factura.\n");
            break;
        }

        mostrar_producto();
        printf("Ingrese el codigo del producto a facturar: ");
        char codigo[7];
        fgets(codigo, sizeof(codigo), stdin);
        codigo[strcspn(codigo, "\n")] = '\0';
        limpiar_buffer();

        int indice_producto = buscar_producto_por_codigo(codigo);
        if (indice_producto == -1) {
            printf("Producto no encontrado.\n");
            continue;
        }

        int cantidad = leer_entero_positivo("Ingrese la cantidad a vender: ");
        if (cantidad > producto[indice_producto].cantidad) {
            printf("No hay suficiente stock. Stock actual: %d\n", producto[indice_producto].cantidad);
            continue;
        }

        // Actualizar stock
        producto[indice_producto].cantidad -= cantidad;

        // Agregar item a la factura con conversiones
        ItemFactura nuevo_item;
        strcpy(nuevo_item.codigo_producto, codigo);
        nuevo_item.cantidad = cantidad;
        nuevo_item.precio_unitario = producto[indice_producto].precio_venta;
        nuevo_item.precio_dolares = producto[indice_producto].precio_venta / tasa_dolar;
        nuevo_item.precio_euros = producto[indice_producto].precio_venta / tasa_euro;
        nuevo_item.precio_pesos = producto[indice_producto].precio_venta / tasa_peso;
        
        nuevo_item.subtotal = cantidad * producto[indice_producto].precio_venta;
        nuevo_item.subtotal_dolares = cantidad * nuevo_item.precio_dolares;
        nuevo_item.subtotal_euros = cantidad * nuevo_item.precio_euros;
        nuevo_item.subtotal_pesos = cantidad * nuevo_item.precio_pesos;

        nueva_factura.items[nueva_factura.num_items] = nuevo_item;
        nueva_factura.num_items++;
        
        // Actualizar subtotales
        nueva_factura.subtotal += nuevo_item.subtotal;
        nueva_factura.subtotal_dolares += nuevo_item.subtotal_dolares;
        nueva_factura.subtotal_euros += nuevo_item.subtotal_euros;
        nueva_factura.subtotal_pesos += nuevo_item.subtotal_pesos;

        printf("Producto agregado a la factura.\n");
        printf("Desea agregar otro producto? (S/N): ");
        scanf(" %c", &continuar);
        limpiar_buffer();
    }

    // Calcular totales con IVA en todas las monedas
    nueva_factura.iva = nueva_factura.subtotal * 0.16; // IVA 16%
    nueva_factura.iva_dolares = nueva_factura.subtotal_dolares * 0.16;
    nueva_factura.iva_euros = nueva_factura.subtotal_euros * 0.16;
    nueva_factura.iva_pesos = nueva_factura.subtotal_pesos * 0.16;
    
    nueva_factura.total = nueva_factura.subtotal + nueva_factura.iva;
    nueva_factura.total_dolares = nueva_factura.subtotal_dolares + nueva_factura.iva_dolares;
    nueva_factura.total_euros = nueva_factura.subtotal_euros + nueva_factura.iva_euros;
    nueva_factura.total_pesos = nueva_factura.subtotal_pesos + nueva_factura.iva_pesos;

    // Guardar factura
    facturas[num_facturas] = nueva_factura;
    num_facturas++;

    // Mostrar factura creada
    mostrar_factura(nueva_factura.numero_factura);
    printf("Factura creada exitosamente!\n");
}    
    //Crear facturas
    void mostrar_factura(int numero_factura) {
    int indice_factura = -1;
    for (int i = 0; i < num_facturas; i++) {
        if (facturas[i].numero_factura == numero_factura) {
            indice_factura = i;
            break;
        }
    }

    if (indice_factura == -1) {
        printf("Factura no encontrada.\n");
        return;
    }

    Factura factura = facturas[indice_factura];

    // Obtener información de cliente y trabajador
    int indice_cliente = buscar_cliente_por_cedula(factura.cedula_cliente);
    int indice_trabajador = buscar_trabajador_por_cedula(factura.cedula_trabajador);

    printf("\n==================================================\n");
    printf("                  FACTURA #%d\n", factura.numero_factura);
    printf("Fecha: %02d/%02d/%d\n", factura.fecha.dia, factura.fecha.mes, factura.fecha.anio);
    printf("Tasas de cambio: USD: %.2f | EUR: %.2f | COP: %.2f\n", tasa_dolar, tasa_euro, tasa_peso);
    printf("==================================================\n");
    
    printf("\nCLIENTE:\n");
    if (indice_cliente != -1) {
        printf("Nombre: %s\n", clien[indice_cliente].nombre);
        printf("Cedula: %d\n", clien[indice_cliente].cedula);
        printf("Telefono: %s\n", clien[indice_cliente].tlf);
        printf("Direccion: %s\n", clien[indice_cliente].direc);
    } else {
        printf("Informacion del cliente no disponible.\n");
    }
    
    printf("\nVENDEDOR:\n");
    if (indice_trabajador != -1) {
        printf("Nombre: %s\n", trab[indice_trabajador].nombre);
        printf("Cedula: %d\n", trab[indice_trabajador].cedula);
    } else {
        printf("Informacion del vendedor no disponible.\n");
    }
    
    printf("\nDETALLE DE COMPRA:\n");
    printf("--------------------------------------------------------------------------------------------------------\n");
    printf("%-10s %-20s %-8s %-12s %-12s %-12s %-12s %-12s\n", 
           "CODIGO", "PRODUCTO", "CANT.", "PRECIO (BS)", "PRECIO (USD)", "PRECIO (EUR)", "PRECIO (COP)", "SUBTOTAL");
    printf("--------------------------------------------------------------------------------------------------------\n");
    
    for (int i = 0; i < factura.num_items; i++) {
        ItemFactura item = factura.items[i];
        int indice_producto = buscar_producto_por_codigo(item.codigo_producto);
        char nombre_producto[60] = "Producto no encontrado";
        
        if (indice_producto != -1) {
            strcpy(nombre_producto, producto[indice_producto].nom);
        }
        
        printf("%-10s %-20s %-8d %-12.2f %-12.2f %-12.2f %-12.2f %-12.2f\n", 
               item.codigo_producto, nombre_producto, item.cantidad, 
               item.precio_unitario, item.precio_dolares, 
               item.precio_euros, item.precio_pesos, 
               item.subtotal);
    }
    
    printf("--------------------------------------------------------------------------------------------------------\n");
    printf("%70s: %12.2f Bs. | %12.2f USD | %12.2f EUR | %12.2f COP\n", 
           "SUBTOTAL", factura.subtotal, factura.subtotal_dolares, 
           factura.subtotal_euros, factura.subtotal_pesos);
    printf("%70s: %12.2f Bs. | %12.2f USD | %12.2f EUR | %12.2f COP\n", 
           "IVA (16%)", factura.iva, factura.iva_dolares, 
           factura.iva_euros, factura.iva_pesos);
    printf("%70s: %12.2f Bs. | %12.2f USD | %12.2f EUR | %12.2f COP\n", 
           "TOTAL", factura.total, factura.total_dolares, 
           factura.total_euros, factura.total_pesos);
    printf("==================================================\n");
    }
    
    //Mostrar factura
    void mostrar_ultimas_facturas() {
    if (num_facturas == 0) {
        printf("No hay facturas registradas.\n");
        return;
    }

    int inicio = (num_facturas > 7) ? num_facturas - 7 : 0;
    
    printf("\n=== ULTIMAS %d FACTURAS ===\n", (num_facturas > 7) ? 7 : num_facturas);
    printf("Tasas de cambio actuales: USD: %.2f | EUR: %.2f | COP: %.2f\n", tasa_dolar, tasa_euro, tasa_peso);
    
    for (int i = inicio; i < num_facturas; i++) {
        printf("\nFactura #%d - Fecha: %02d/%02d/%d\n", 
               facturas[i].numero_factura, 
               facturas[i].fecha.dia, 
               facturas[i].fecha.mes, 
               facturas[i].fecha.anio);
        
        printf("Total: %.2f Bs. | %.2f USD | %.2f EUR | %.2f COP\n",
               facturas[i].total, facturas[i].total_dolares,
               facturas[i].total_euros, facturas[i].total_pesos);
        
        int indice_cliente = buscar_cliente_por_cedula(facturas[i].cedula_cliente);
        if (indice_cliente != -1) {
            printf("Cliente: %s (C.I. %d)\n", clien[indice_cliente].nombre, clien[indice_cliente].cedula);
        }
        
        printf("--------------------------------------------------\n");
    }
}   
    
    //tazas de cambio
    void actualizar_tasas_cambio() {
    printf("\n--- ACTUALIZAR TASAS DE CAMBIO ---\n");
    printf("Tasa actual del dolar: %.2f Bs.\n", tasa_dolar);
    tasa_dolar = leer_flotante_positivo("Ingrese nueva tasa del dolar: ");
    
    printf("Tasa actual del euro: %.2f Bs.\n", tasa_euro);
    tasa_euro = leer_flotante_positivo("Ingrese nueva tasa del euro: ");
    
    printf("Tasa actual del peso colombiano: %.2f Bs.\n", tasa_peso);
    tasa_peso = leer_flotante_positivo("Ingrese nueva tasa del peso colombiano: ");
    
    printf("Tasas de cambio actualizadas exitosamente!\n");
}


//-------------------------------------------------

// FUNCIONES CREAR, M0STRAR DATOS
void leer_producto(int cantidad_a_agregar){
	if (cantidad_a_agregar <= 0) {
        printf("Error: La cantidad de productos a agregar debe ser positiva.\n");
        return;
    }
    
    if ((num_productos + cantidad_a_agregar) > MAX_PRODUCT) {
        printf("Error: No se pueden agregar %d productos.", cantidad_a_agregar);
        return;
    }
    
	for(int i=0;i<cantidad_a_agregar;i++){
	 int j = num_productos + i;
	  printf("Ingrese el nombre del zapato\n");
	  fgets(producto[j].nom,sizeof(producto[j].nom),stdin);
	  producto[j].nom[strcspn(producto[j].nom,"\n")]='\0';
	  printf("Ingrese una descripcion para el producto: ");
	  fgets(producto[j].desc, sizeof(producto[j].desc), stdin);
      producto[j].desc[strcspn(producto[j].desc, "\n")] = '\0';
      
      do {
            printf("Ingrese el tipo de zapato:\n(1 para deportivo; 2 para casual; 3 para formal): ");
            scanf("%d", &producto[j].tipo);
            limpiar_buffer();
            if (producto[j].tipo < 1 || producto[j].tipo > 3) {
                printf("Opcion invalida. Por favor, ingrese 1, 2 o 3.\n");
            }
        } while (producto[j].tipo < 1 || producto[j].tipo > 3);
        
        do {
            printf("El producto tiene alguna condicion?\n(1 para buen estado; 2 para mal estado): ");
            scanf("%d", &producto[j].categoria);
            limpiar_buffer();
            if (producto[j].categoria < 1 || producto[j].categoria > 2) {
                printf("Opcion invalida. Por favor, ingrese 1 o 2.\n");
            }
        } while (producto[j].categoria < 1 || producto[j].categoria > 2);
		
        producto[j].cantidad = leer_entero_positivo("Ingrese la cantidad de este producto: ");
        printf("Ingrese el costo de adquisicion del producto: ");
        scanf("%f", &producto[j].costo);
        limpiar_buffer();
        printf("Ingrese el precio de venta del producto: ");
        scanf("%f", &producto[j].precio_venta);
        limpiar_buffer();
        
        printf("Ingrese la fecha adquisicion: \n");
        do {
            printf("Dia (1-31): ");
            scanf("%d", &producto[j].fecha_adquisicion.dia); 
            limpiar_buffer();
            printf("Mes (1-12): ");
            scanf("%d", &producto[j].fecha_adquisicion.mes);
            limpiar_buffer();
            printf("Anio: ");
            scanf("%d", &producto[j].fecha_adquisicion.anio); 
            limpiar_buffer();

            if (!validar_fecha(producto[j].fecha_adquisicion.dia, producto[j].fecha_adquisicion.mes, producto[j].fecha_adquisicion.anio)) {
                printf("Fecha invalida. Por favor, ingrese una fecha valida.\n");
            }
        } while (!validar_fecha(producto[j].fecha_adquisicion.dia, producto[j].fecha_adquisicion.mes, producto[j].fecha_adquisicion.anio));
        
        producto[j].codigo[0] = 'Z'; 
        switch(producto[j].tipo){
            case 1: producto[j].codigo[1] = '1'; break;
            case 2: producto[j].codigo[1] = '2'; break;
            case 3: producto[j].codigo[1] = '3'; break;
        }
        switch(producto[j].categoria){
            case 1: producto[j].codigo[2] = 'A'; break;
            case 2: producto[j].codigo[2] = 'B'; break;
        }
        
        if((j+1)<10){ // Si el numero es del 1 al 9
            producto[j].codigo[3]='0';
            producto[j].codigo[4]='0';
            producto[j].codigo[5]='0'+(j+1);
            producto[j].codigo[6]='\0';
        }else if((j+1)<100){ // Si el numero es del 10 al 99
            producto[j].codigo[3]='0';
            sprintf(&producto[j].codigo[4],"%d",j+1);
            producto[j].codigo[6]='\0';
        } else { // Si el numero es 100 o ms
            sprintf(&producto[j].codigo[3],"%d",j+1);
            producto[j].codigo[6] = '\0';
        }
        printf("Codigo generado para el producto: %s\n", producto[j].codigo);
    }
    num_productos += cantidad_a_agregar; 
    printf("\nSe agregaron %d producto(s) con exito!", cantidad_a_agregar);
}
    void mostrar_producto(){
    if (num_productos == 0) {
        printf("No hay productos registrados en el sistema.\n");
        return;
    }

    printf("\n--- LISTADO DE PRODUCTOS ---\n");
    printf("%-20s\t%-15s\t%-15s\t%-10s\t%-12s\t%-12s\t%-12s\t%-12s\t%-12s\t%-15s\t%-10s\n",
           "Nombre", "Tipo", "Condicion", "Cantidad", "Costo (BS)", "Venta (BS)",
           "Venta (USD)", "Venta (EUR)", "Venta (COP)", "Adquisicion", "Codigo");
    printf("------------------------------------------------------------------------------------------------------------------------------------------------------------------\n"); // Ajustar l�nea de separaci�n

    for(int i = 0; i < num_productos; i++){
        printf("%-20s\t", producto[i].nom);

        switch(producto[i].tipo){
            case 1: printf("%-15s\t","Deportivo"); break;
            case 2: printf("%-15s\t","Casual"); break;
            case 3: printf("%-15s\t","Formal"); break;
            default: printf("%-15s\t","N/D"); break;
        }
        switch(producto[i].categoria){
            case 1: printf("%-15s\t","Buen estado"); break;
            case 2: printf("%-15s\t","Mal estado"); break;
            default: printf("%-15s\t","N/D"); break;
        }

        printf("%-10d\t", producto[i].cantidad);

        printf("%-12.2f\t%-12.2f\t",
               producto[i].costo, producto[i].precio_venta);

        printf("%-12.2f\t", producto[i].precio_venta / tasa_dolar);

        printf("%-12.2f\t", producto[i].precio_venta / tasa_euro);

        printf("%-12.2f\t", producto[i].precio_venta / tasa_peso);
 
        printf("%02d/%02d/%d\t",
               producto[i].fecha_adquisicion.dia,
               producto[i].fecha_adquisicion.mes,
               producto[i].fecha_adquisicion.anio);

        printf("%-10s\n", producto[i].codigo);
    }
    printf("------------------------------------------------------------------------------------------------------------------------------------------------------------------\n"); // Ajustar l�nea de separaci�n
}

void leer_trabajador(int cantidad_a_agregar){
if (cantidad_a_agregar <= 0) {
        printf("Error: La cantidad de trabajadores a agregar debe ser positiva.\n");
        return;
    }
    
    if ((num_trabajadores + cantidad_a_agregar) > MAX_WORKERS) {
        printf("Error: No se pueden agregar %d trabajadores.\n", cantidad_a_agregar);
        return;
    }
    
   for(int i=0;i< cantidad_a_agregar;i++){
   	int j = num_trabajadores + i;
    printf("Ingrese el nombre del trabajador: ");
    fgets(trab[j].nombre,sizeof(trab[j].nombre),stdin);
    trab[j].nombre[strcspn(trab[j].nombre,"\n")]='\0';
    trab[j].cedula = leer_entero_positivo("Ingrese la C.I del trabajador: ");
    printf("Ingrese numero de telefono del trabajador: ");
    fgets(trab[j].tlf,sizeof(trab[j].tlf),stdin);
    trab[j].tlf[strcspn(trab[j].tlf,"\n")]='\0';
    printf("Ingrese la direccion del trabajador: ");
    fgets(trab[j].direc,sizeof(trab[j].direc),stdin);
    trab[j].direc[strcspn(trab[j].direc,"\n")]='\0';
   }
    num_trabajadores += cantidad_a_agregar;
    printf("\nSe agregaron %d trabajador(es) con exito!\n", cantidad_a_agregar);
}

void mostrar_trabajador() { 
    if (num_trabajadores == 0) {
        printf("\nNo hay trabajadores registrados en el sistema.\n");
        return;
    }
     printf("\n--- LISTADO DE TRABAJADORES ---\n");
    printf("%-5s\t%-20s\t%-10s\t%-20s\t%-30s\n", "Nro.", "Nombre", "Cedula", "Telefono", "Direccion");
    printf("--------------------------------------------------------------------------------------------------\n"); // Ajustamos la linea separadora

    for (int i = 0; i < num_trabajadores; i++) {
        printf("%-5d\t", i + 1); // nUmero de empleado (indice + 1)
        printf("%-20s\t%-10d\t%-20s\t%-30s\n",
               trab[i].nombre, trab[i].cedula, trab[i].tlf, trab[i].direc);
    }
    printf("--------------------------------------------------------------------------------------------------\n"); // Ajustamos la linea separadora
}

void leer_cliente(int cantidad_a_agregar){
	if (cantidad_a_agregar <= 0) {
        printf("Error: La cantidad de clientes a agregar debe ser positiva.\n");
        return;
    }
    if ((num_clientes + cantidad_a_agregar) > MAX_CLIENTS) {
        printf("Error: No se pueden agregar %d clientes.\n", cantidad_a_agregar);
        return;
    }
    for(int i=0;i<cantidad_a_agregar;i++){
    	int j = num_clientes + i;
    printf("Ingrese el nombre del cliente: ");
    fgets(clien[j].nombre,sizeof(clien[j].nombre),stdin);
     clien[j].nombre[strcspn(clien[j].nombre, "\n")] ='\0';
    clien[j].cedula = leer_entero_positivo("Ingrese la C.I del cliente: ");
    printf("Ingrese numero de telefono del cliente: ");
    fgets(clien[j].tlf,sizeof(clien[j].tlf),stdin);
    clien[j].tlf[strcspn(clien[j].tlf, "\n")]='\0';
    printf("Ingrese la direccion del cliente: ");
    fgets(clien[j].direc,sizeof(clien[j].direc),stdin);
    clien[j].direc[strcspn(clien[j].direc,"\n")]='\0';
   }
  	num_clientes += cantidad_a_agregar;
	printf("\nSe agregaron %d cliente(s) con exito! \n", cantidad_a_agregar);
}

void mostrar_cliente(){
	if(num_clientes == 0){
		printf("\nNo hay clientes registrados.\n");
		return;
	}
	printf("\n--- LISTADO DE CLIENTES ---\n");
     printf("%-20s\t%-15s\t%-20s\t%-30s\n", "Nombre", "Cedula", "Telefono", "Direccion");
     printf("----------------------------------------------------------------------------------\n");
    for (int i = 0; i < num_clientes;i++) {
         printf("%-20s\t%-15d\t%-20s\t%-30s\n", 
               clien[i].nombre,clien[i].cedula,clien[i].tlf,clien[i].direc);
}
printf("----------------------------------------------------------------------------------\n");
}

void leer_proveedor(int cantidad_a_agregar){
	if ((num_proveedores + cantidad_a_agregar) > MAX_SUPPLIERS) {
        printf("Error: No se pueden agregar %d proveedores\n",
               cantidad_a_agregar);
        return; 
    }
    
    if (cantidad_a_agregar <= 0) {
        printf("Cantidad de proveedores a agregar debe ser positiva.\n");
        return;
    }

     for(int i=0;i<cantidad_a_agregar;i++){
    	int j = num_proveedores + i;
    printf("Ingrese el nombre del proveedor: ");
    fgets(prov[j].nombre, sizeof(prov[j].nombre),stdin);
    prov[j].nombre[strcspn(prov[j].nombre,"\n")]='\0';
    prov[j].cedula = leer_entero_positivo("Ingrese la C.I del proveedor: ");
    printf("Ingrese numero de contacto del proveedor: ");
    fgets(prov[j].tlf, sizeof(prov[j].tlf),stdin);
    prov[j].tlf[strcspn(prov[j].tlf,"\n")]='\0';
    printf("Ingrese la direccion del proveedor: ");
        fgets(prov[j].direc, sizeof(prov[j].direc), stdin);
        prov[j].direc[strcspn(prov[j].direc,"\n")]='\0';
   }
   num_proveedores += cantidad_a_agregar;
    printf("\nSe agregaron %d proveedor(es) con exito!\n", cantidad_a_agregar);
}

void mostrar_proveedor(){
	if (num_proveedores == 0) {
        printf("\nNo hay proveedores registrados en el sistema.\n");
        return;
    }
    printf("\n--- LISTADO DE PROVEEDORES ---\n");
    printf("%-20s\t%-10s\t%-20s\t%-30s\n", "Nombre", "Cedula", "Telefono", "Direccion");
    printf("----------------------------------------------------------------------------------\n");
    for (int i = 0; i < num_proveedores; i++) {
         printf("%-20s\t%-10d\t%-20s\t%-30s\n",
               prov[i].nombre, prov[i].cedula, prov[i].tlf, prov[i].direc);
	}
	printf("----------------------------------------------------------------------------------\n");
}

// FUNCIONES MODIFICAR Y ELIMINAR DATOS
void modificar_trabajador(){
	if (num_trabajadores == 0) {
        printf("Actualmente no hay trabajadores registrados para modificar.\n");
        return;
    }
    int cedula_buscar;
    printf("\n--- Modificar Trabajador ---\n");
    cedula_buscar = leer_entero_positivo("Ingrese la cedula del trabajador a modificar: ");
    int indice = buscar_trabajador_por_cedula(cedula_buscar);
    if (indice == -1) {
        printf("Error: No se encontro un trabajador con la cedula %d.\n", cedula_buscar);
        return;
    }
            printf("Modificando trabajador: %s\n", trab[indice].nombre);
            
            printf("Nuevo nombre (actual: %s): ", trab[indice].nombre);
            fgets(trab[indice].nombre, sizeof(trab[indice].nombre), stdin);
            trab[indice].nombre[strcspn(trab[indice].nombre, "\n")] = '\0';
            
            printf("Nuevo telefono (actual: %s): ", trab[indice].tlf);
            fgets(trab[indice].tlf, sizeof(trab[indice].tlf), stdin);
            trab[indice].tlf[strcspn(trab[indice].tlf, "\n")] = '\0';
            
            printf("Nueva direccion (actual: %s): ", trab[indice].direc);
            fgets(trab[indice].direc, sizeof(trab[indice].direc), stdin);
            trab[indice].direc[strcspn(trab[indice].direc, "\n")] = '\0';
            
            printf("Trabajador modificado con exito\n");
    }

void eliminar_trabajador(){
	printf("\n--- Eliminar trabajador ---\n");
	if (num_trabajadores == 0) {
        printf("Actualmente no hay trabajadores registrados para eliminar.\n");
        return;
    }

    int cedula_a_eliminar;
    cedula_a_eliminar = leer_entero_positivo("Ingrese la cedula del trabajador que desea eliminar: ");
    int indice = buscar_trabajador_por_cedula(cedula_a_eliminar);

    if (indice == -1) {
        printf("Error: No se encontro un trabajador con la cedula %d.\n", cedula_a_eliminar);
        return;
    }

    printf("\nTrabajador encontrado. Se eliminaran los siguientes datos:\n");
    printf("  Nombre: %s\n", trab[indice].nombre);
    printf("  Cedula: %d\n", trab[indice].cedula);
    printf("  Telefono: %s\n", trab[indice].tlf);
    printf("  Direccion: %s\n", trab[indice].direc);

    char confirmacion;
    printf("�Esta completamente seguro de que desea eliminar a este trabajador? (S/N): ");
    // El espacio antes de %c es importante para consumir cualquier caracter de nueva linea
    // que haya quedado en el buffer de entrada de una lectura anterior (ej. de leer_entero_positivo)
    scanf(" %c", &confirmacion); 

    if (confirmacion == 'S' || confirmacion == 's') {
        //  eliminar el elemento es pq se mueven todos los elementos desde la posicion siguiente
        // una posicion hacia atras, sobrescribiendo el elemento actual
        for (int i = indice; i < num_trabajadores - 1; i++) {
            trab[i] = trab[i+1]; 
        }
        num_trabajadores--; 
        printf("�Trabajador eliminado con exito!\n");
    } else {
        printf("Eliminacion cancelada. El trabajador no ha sido removido.\n");
    }
    }
	void modificar_cliente(){
	if (num_clientes == 0) {
        printf("Actualmente no hay clientes registrados para modificar.\n");
        return;
    }
    int cedula_buscar;
    printf("\n--- Modificar Cliente ---\n");
    cedula_buscar = leer_entero_positivo("Ingrese la cedula del cliente que desea modificar: ");
    int indice = buscar_cliente_por_cedula(cedula_buscar);
    if (indice == -1) {
        printf("Error: No se encontro un cliente con la cedula %d.\n", cedula_buscar);
        return;
    }
            printf("Modificando cliente: %s\n", clien[indice].nombre);
            
            printf("Nuevo nombre (actual: %s): ", clien[indice].nombre);
            fgets(clien[indice].nombre, sizeof(clien[indice].nombre), stdin);
            clien[indice].nombre[strcspn(clien[indice].nombre, "\n")] = '\0';
            
            printf("Nuevo telefono (actual: %s): ", clien[indice].tlf);
            fgets(clien[indice].tlf, sizeof(clien[indice].tlf), stdin);
            clien[indice].tlf[strcspn(clien[indice].tlf, "\n")] = '\0';
            
            printf("Nueva direccion (actual: %s): ", clien[indice].direc);
            fgets(clien[indice].direc, sizeof(clien[indice].direc), stdin);
            clien[indice].direc[strcspn(clien[indice].direc, "\n")] = '\0';
            
            printf("Cliente modificado con exito\n");
	}

	void eliminar_cliente(){
	printf("\n--- Eliminar Cliente ---\n");
	if (num_clientes == 0) {
        printf("Actualmente no hay clientes registrados para eliminar.\n");
        return;
    }

    int cedula_a_eliminar;
    cedula_a_eliminar = leer_entero_positivo("Ingrese la cedula del clientes que desea eliminar: ");
    int indice = buscar_cliente_por_cedula(cedula_a_eliminar);

    if (indice == -1) {
        printf("Error: No se encontro un cliente con la cedula %d.\n", cedula_a_eliminar);
        return;
    }

    printf("\nCliente encontrado. Se eliminaran los siguientes datos:\n");
    printf("  Nombre: %s\n", clien[indice].nombre);
    printf("  Cedula: %d\n", clien[indice].cedula);
    printf("  Telefono: %s\n", clien[indice].tlf);
    printf("  Direccion: %s\n", clien[indice].direc);

    char confirmacion;
    printf("�Esta completamente seguro de que desea eliminar a este cliente? (S/N): ");
    
    scanf(" %c", &confirmacion); 

    if (confirmacion == 'S' || confirmacion == 's') {
        for (int i = indice; i < num_clientes - 1; i++) {
            clien[i] = clien[i+1]; 
        }
        num_clientes--; 
        printf("�Cliente eliminado con exito!\n");
    } else {
        printf("Eliminacion cancelada. El cliente no ha sido removido.\n");
    }
}

	void modificar_producto(){
		if (num_productos == 0) {
        printf("Actualmente no hay productos registrados para modificar.\n");
        return;
    }
     char codigo_buscar[7];
      printf("Ingrese el codigo del producto a modificar: ");
    fgets(codigo_buscar, sizeof(codigo_buscar), stdin);
    codigo_buscar[strcspn(codigo_buscar, "\n")] = '\0';
    limpiar_buffer();
    printf("\n--- Modificar Producto ---\n");
    int indice = buscar_producto_por_codigo(codigo_buscar);
    if (indice == -1) {
         printf("Error: No se encontro un producto con el codigo '%s'.\n", codigo_buscar);
        return;
    }
            printf("Modificando Producto: %s\n", producto[indice].nom);
            
        printf("  Nuevo nombre (actual: %s): ", producto[indice].nom);
    	fgets(producto[indice].nom, sizeof(producto[indice].nom), stdin);
    	producto[indice].nom[strcspn(producto[indice].nom, "\n")] = '\0';
        printf("  Nueva descripcion (actual: %s): ", producto[indice].desc);
    	fgets(producto[indice].desc, sizeof(producto[indice].desc), stdin);
   		producto[indice].desc[strcspn(producto[indice].desc, "\n")] = '\0';
    	do {
        printf("  Nuevo tipo (actual: %d - 1: Deportivo, 2: Casual, 3: Formal): ", producto[indice].tipo);
        scanf("%d", &producto[indice].tipo);
        limpiar_buffer();
        if (producto[indice].tipo < 1 || producto[indice].tipo > 3) {
            printf("Opcion invalida. Por favor, ingrese 1, 2 o 3.\n");
        }
    	} while (producto[indice].tipo < 1 || producto[indice].tipo > 3);
		do {
        printf("  Nueva categoria (actual: %d - 1: Buen estado, 2: Mal estado): ", producto[indice].categoria);
        scanf("%d", &producto[indice].categoria);
        limpiar_buffer();
        if (producto[indice].categoria < 1 || producto[indice].categoria > 2) {
            printf("Opcion invalida. Por favor, ingrese 1 o 2.\n");
        }
    	} while (producto[indice].categoria < 1 || producto[indice].categoria > 2);
		printf("  Nueva cantidad (actual: %d): ", producto[indice].cantidad);
    	producto[indice].cantidad = leer_entero_no_negativo("Ingrese la nueva cantidad: ");
		printf("  Nuevo costo (actual: %.2f): ", producto[indice].costo);
    	producto[indice].costo = leer_flotante_positivo("Ingrese el nuevo costo: ");
 		printf("  Nuevo precio de venta (actual: %.2f): ", producto[indice].precio_venta);
    	producto[indice].precio_venta = leer_flotante_positivo("Ingrese el nuevo precio de venta: ");
	
    printf("  Nueva fecha de adquisicion (actual: %02d/%02d/%d):\n",
           producto[indice].fecha_adquisicion.dia,
           producto[indice].fecha_adquisicion.mes,
           producto[indice].fecha_adquisicion.anio);
    do {
        printf("    Dia (1-31): ");
        scanf("%d", &producto[indice].fecha_adquisicion.dia);
        limpiar_buffer();
        printf("    Mes (1-12): ");
        scanf("%d", &producto[indice].fecha_adquisicion.mes);
        limpiar_buffer();
        printf("    Anio: ");
        scanf("%d", &producto[indice].fecha_adquisicion.anio);
        limpiar_buffer();

        if (!validar_fecha(producto[indice].fecha_adquisicion.dia, producto[indice].fecha_adquisicion.mes, producto[indice].fecha_adquisicion.anio)) {
            printf("Fecha invalida. Por favor, ingrese una fecha valida.\n");
        }
    } while (!validar_fecha(producto[indice].fecha_adquisicion.dia, producto[indice].fecha_adquisicion.mes, producto[indice].fecha_adquisicion.anio));

    char temp_codigo[7]; 
    temp_codigo[0] = 'Z';
    
    switch(producto[indice].tipo){
        case 1: temp_codigo[1] = '1'; break;
        case 2: temp_codigo[1] = '2'; break;
        case 3: temp_codigo[1] = '3'; break;
    }
    switch(producto[indice].categoria){
        case 1: temp_codigo[2] = 'A'; break;
        case 2: temp_codigo[2] = 'B'; break;
    }
    if((indice+1)<10){ 
        temp_codigo[3]='0';
        temp_codigo[4]='0';
        temp_codigo[5]='0'+(indice+1);
        temp_codigo[6]='\0';
    }else if((indice+1)<100){ 
        temp_codigo[3]='0';
        sprintf(&temp_codigo[4],"%d",indice+1);
        temp_codigo[6]='\0';
    } else { 
        sprintf(&temp_codigo[3],"%d",indice+1);
        temp_codigo[6] = '\0';
    }
    strcpy(producto[indice].codigo, temp_codigo); 

    printf("Producto modificado con exito. Nuevo codigo: %s\n", producto[indice].codigo);
	}
	
	void eliminar_producto(){
	printf("\n--- Eliminar Producto ---\n");
	if (num_productos == 0) {
        printf("Actualmente no hay productos registrados para eliminar.\n");
        return;
    }

    char codigo_a_eliminar[7];
    
	printf("Ingrese el codigo del producto que desea eliminar: ");
    fgets(codigo_a_eliminar, sizeof(codigo_a_eliminar), stdin);
    codigo_a_eliminar[strcspn(codigo_a_eliminar, "\n")] = '\0';

    int indice = buscar_producto_por_codigo(codigo_a_eliminar); 

    if (indice == -1) {
        printf("Error: No se encontro un producto con el codigo '%s'.\n", codigo_a_eliminar);
        return;
    }

    printf("\nProducto encontrado. Se eliminaran los siguientes datos:\n");
    printf("  Codigo: %s\n", producto[indice].codigo);
    printf("  Nombre: %s\n", producto[indice].nom);
    printf("  Descripcion: %s\n", producto[indice].desc);
    printf("  Cantidad: %d\n", producto[indice].cantidad);
    printf("  Precio de Venta: %.2f\n", producto[indice].precio_venta);
    printf("  Fecha de Adquisicion: %02d/%02d/%d\n",
       producto[indice].fecha_adquisicion.dia,
       producto[indice].fecha_adquisicion.mes,
       producto[indice].fecha_adquisicion.anio);

    char confirmacion;
    printf("�Esta completamente seguro de que desea eliminar este producto? (S/N): ");
    scanf(" %c", &confirmacion); 

    if (confirmacion == 'S' || confirmacion == 's') {
        for (int i = indice; i < num_productos - 1; i++) {
            producto[i] = producto[i+1];
        }
        num_productos--;
        printf("�Producto eliminado con exito!\n");
    } else {
        printf("Eliminacion cancelada. El producto no ha sido removido.\n");
    }
}

void limpiar_buffer() {
    int c;
    while ((c = getchar()) != '\n' && c != EOF);
}

int buscar_trabajador_por_cedula(int cedula_a_buscar) {
    for (int i = 0; i < num_trabajadores; i++) {
        if (trab[i].cedula == cedula_a_buscar) {
            return i; 
        }
    }
    return -1; 
}


int buscar_cliente_por_cedula(int cedula_a_buscar) {
    for (int i = 0; i < num_clientes; i++) {
        if (clien[i].cedula == cedula_a_buscar) {
            return i; 
        }
    }
    return -1;
}

int buscar_producto_por_codigo(const char *codigo_a_buscar) {
    for (int i = 0; i < num_productos; i++) {
        if (strcmp(producto[i].codigo, codigo_a_buscar) == 0) {
            return i;
        }
    }
    return -1;
}

int leer_entero_positivo(const char *mensaje) { 
    int valor;
    int lectura;
    do {
        printf("%s", mensaje);
        lectura = scanf("%d", &valor);
        limpiar_buffer();
        
        if (lectura != 1) {
            printf("Error: Ingrese numeros enteros.\n");
            limpiar_buffer();
        } else if (valor <= 0){
			printf("Error: Ingrese un numero positivo mayor que cero.\n");
			limpiar_buffer();
        }
		} while (lectura != 1 || valor <= 0);
    return valor;
}

int leer_entero_no_negativo(const char *mensaje) {
    int valor;
    int lectura;
    do {
        printf("%s", mensaje);
        lectura = scanf("%d", &valor); 
        limpiar_buffer(); 

        if (lectura != 1) { 
            printf("Error: Por favor, ingrese un numero entero valido.\n");
        } else if (valor < 0) { 
            printf("Error: Ingrese un numero entero que sea cero o positivo.\n");
        }
    } while (lectura != 1 || valor < 0); 
    return valor;
}

float leer_flotante_positivo(const char *mensaje){
    float valor;
    int lectura;
    do {
        printf("%s", mensaje);
        lectura = scanf("%f", &valor); 
        limpiar_buffer(); 

        if (lectura != 1) {
            printf("Error: Por favor, ingrese un numero decimal valido.\n");
        } else if (valor <= 0) {
            printf("Error: Ingrese un numero positivo mayor que cero.\n"); 
        }
    } while (lectura != 1 || valor <= 0);
    return valor;
}

  bool validar_fecha(int dia, int mes, int anio) {
    if (anio < 1900 || anio > 2025) { 
        return false;
    }
    if (mes < 1 || mes > 12) {
        return false;
    }
    if (dia < 1 || dia > 31) {
        return false;
    }
    int dias_en_mes[] = {0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
    if ((anio % 400 == 0) || (anio % 4 == 0 && anio % 100 != 0)) {
        dias_en_mes[2] = 29;
    }
    if (dia > dias_en_mes[mes]) {
        return false;
    }
    return true;
}
    