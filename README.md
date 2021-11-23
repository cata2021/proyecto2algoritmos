*proyecto2algoritmos*
*Algoritmos II Proyecto II Semestre 2021 TEC / ATI*

#include <stdio.h>
#include <stdlib.h>

typedef struct NodoC NodoC;
typedef struct Arista Arista;
typedef struct Colaborador Colaborador;
typedef struct ListaB ListaBitacora;
typedef struct NodoB NodoB;
typedef struct Bitacora Bitacora;




*Estructuras de colaborador*
struct Colaborador
{
	int cedula;
	char nombre[100];          //Estructura de colaborador
	char apellidos[100];
	char correo[100]; 
	char rol[100];
	char fechaC[100];
};

struct Chat{
	char remitente[100];
	char destinatario[100];
	char mensaje[1500];
};

struct NodoC{
	Colaborador colaborador;
	NodoC *siguiente;
	Arista *adyacencia;
};

struct ListaChat{
	NodoM *inicio;
	NodoM *final;
};

struct Arista{
	NodoC *vrt;
	Arista *siguiente;
};

*Estructuras del Chat* 
struct Bitacora
{
	int cedula;
	char fecha[10];
	char nombreCompleto[150];
	char detalle[150];	
};

struct NodoB
{
	Bitacora bitacora; 
	NodoB *siguiente;
	NodoB *anterior; 
};

struct ListaB
{
	NodoB *inicio;
	NodoB *final;
};

*Estructuras de equipo*
typedef struct equipo{         
	char nombreE[70];
	char descripcionE[200];
	int cantidadC;
} equipo;

struct NodoE{
	equipo equipo;     
	NodoE *siguiente;
};

struct ListaE{
	NodoE *inicio;
	NodoE *final;
};

NodoC *inicio = NULL;  //Nodo inicial grafo

void insertarChat(ListaChat *L, Chat chat){
    NodoM *nuevo= crearNodo(chat);
    NodoM *n, *aux;
    if(L->inicio == NULL){
        L->inicio = nuevo;
        L->inicio->siguiente = NULL;
        return;
    }
    n = L->inicio;
    while(n!= NULL) {
        aux = n;
        n = n->siguiente;
    }
    aux->siguiente = nuevo;
    aux->siguiente->siguiente = NULL;
}



*Funciones*
void registrarColaborador();
void insertarArista(int cedulas[100], int cantidad);
void agregarArista(NodoC* aux,NodoC* aux2,Arista* nuevo, Arista* nuevo2);
void visualizarGrafo();
void insertarEquipo(ListaE *L);
ListaE *listaNuevaE(void);
void registrarBitacora(ListaBitacora *L);
void guardarBitacora(ListaBitacora *L);
void consultarBitacora(ListaBitacora *L);
void cargarBitacora(ListaBitacora *L);



/* ----------------------------------------------------------------------------------------------------------------------------*/ 
//                                              Funciones de registro de colaboradores
/* ----------------------------------------------------------------------------------------------------------------------------*/
void registrarColaborador()  //Insertar nodo
{
	NodoC *aux;
	NodoC *nuevo = (NodoC*)malloc(sizeof(NodoC));
	fflush(stdin);
	printf("Indique la cedula del colaborador: \n");
	scanf("%d",&nuevo->colaborador.cedula);
	getchar();
	printf("Indique el nombre del colaborador: \n");  
	gets(nuevo->colaborador.nombre);
	printf("Indique los apellidos del colaborador: \n");
	gets(nuevo->colaborador.apellidos);
	printf("Indique el correo del colaborador: \n");
	gets(nuevo->colaborador.correo);
	printf("Indique su rol en la empresa: \n");
	gets(nuevo->colaborador.rol);
	printf("Indique la fecha de cumpleanos del colaborador: \n");
	gets(nuevo->colaborador.fechaC);
	nuevo->siguiente = NULL;
	nuevo->adyacencia = NULL;
	if (inicio == NULL){
		inicio = nuevo;
	}
	else{
		aux = inicio;
		while(aux->siguiente!=NULL){
			aux = aux->siguiente;
		}
		aux->siguiente = nuevo;
	}
}

void agregarArista(NodoC* aux,NodoC* aux2,Arista* nuevo, Arista* nuevo2)
{
    Arista* a, *b;
    NodoC *aux3 = aux;
    if(aux->adyacencia==NULL){   
	    aux->adyacencia=nuevo;
        nuevo->vrt=aux2;
        nuevo->siguiente = NULL;
    }else{   
	    a=aux->adyacencia;
        while(a->siguiente!=NULL){
        	a=a->siguiente;
		}
        nuevo->vrt=aux2;
        a->siguiente=nuevo;
        nuevo->siguiente = NULL;
    }
    if(aux2->adyacencia==NULL){   
	    aux2->adyacencia=nuevo2;
        nuevo2->vrt=aux3;
        nuevo2->siguiente = NULL;
    }else{   
	    b=aux2->adyacencia;
        while(b->siguiente!=NULL){
        	b=b->siguiente;
		}
        nuevo2->vrt=aux3;
        b->siguiente=nuevo2;
        nuevo2->siguiente = NULL;
    }
}



void insertarArista(int cedulas[100], int cantidad)
{
	Arista* ar;
	int cedula, cedula2;
	NodoC *aux, *aux2;
	int i=0,j=0,k,contador;
	contador = cantidad-1;
	if(cantidad == 1)
		return;
	while(j != contador){
		i=j+1;
		while(i <= contador){
			Arista* nuevo=(Arista*)malloc(sizeof(Arista));
			nuevo->siguiente = NULL;
			Arista* nuevo2=(Arista*)malloc(sizeof(Arista));///////////////
			nuevo2->siguiente = NULL;
			NodoC *aux, *aux2;
			if(inicio == NULL){
				printf("Eror: el grafo esta vacio\n");///////////////
				return;
			}
			fflush(stdin);
			cedula= cedulas[j];
			cedula2= cedulas[i];
		    //printf("\n");
			aux = inicio;
			aux2 = inicio;
			while(aux2 != NULL){
				if(aux2->colaborador.cedula == cedula2)
					break;
				aux2 = aux2->siguiente;
			}
			if(aux2 == NULL){
				printf("Error: Vertice no se encontro\n");///////////////
				//return;
			}
			else{
				while(aux != NULL){
					if(aux->colaborador.cedula == cedula){
						ar = aux->adyacencia;
						for(ar; ar != NULL; ar= ar->siguiente){
							if(ar->vrt->colaborador.cedula == aux2->colaborador.cedula){
								break;
							}
						}
						if(ar == NULL)
							agregarArista(aux,aux2,nuevo,nuevo2);
						break;
					}
					aux = aux->siguiente;
				}
				if(aux == NULL){
					printf("Error: Vertice no se encontro\n");
					//return;
				}
			}
			i++;
		}
		j++;
	}
}

void visualizarGrafo(){
    NodoC* aux=inicio;
    Arista* ar;
    printf("Nodos\n");
    while(aux!=NULL){   
	    printf("%s:    ", aux->colaborador.nombre);
        if(aux->adyacencia!=NULL){
            ar=aux->adyacencia;
            while(ar!=NULL){ 
			    printf(" %s",ar->vrt->colaborador.nombre);
                ar=ar->siguiente;
            }
        }
        printf("\n");
        aux=aux->siguiente;
    }
    printf("\n");
}

/* ----------------------------------------------------------------------------------------------------------------------------*/ 
//                                              Funciones de registro de equipo
/* ----------------------------------------------------------------------------------------------------------------------------*/
ListaE *listaNuevaE(void)
{
	ListaE *L;
	L = (ListaE *) malloc(sizeof(ListaE));
	L->inicio = NULL;
	L->final = NULL;
	return L;
}

NodoE* crearNodo(equipo equipo) {
	NodoE *nuevo;
	nuevo = (NodoE *) malloc(sizeof(NodoE));
	nuevo->siguiente = NULL;
	nuevo->equipo = equipo;	 
	return nuevo;
}

void insertarEquipo(ListaE *L, equipo equipo){
    NodoE *nuevo= crearNodo(equipo);
    NodoE *n, *aux;
    if(L->inicio == NULL){
        L->inicio = nuevo;
        L->inicio->siguiente = NULL;
        return;
    }
    n = L->inicio;
    while(n!= NULL) {
        aux = n;
        n = n->siguiente;
    }
    aux->siguiente = nuevo;
    aux->siguiente->siguiente = NULL;
}

void registrarEquipo(ListaE *L){
	equipo nuevoEquipo;
	int cedulaE[50];
	int registroE, i;
	printf("Ingrese el nombre del equipo que desea crear: ");
	gets(nuevoEquipo.nombreE);
	printf("\n");
	printf("Ingrese una breve descripcion del equipo: ");
	gets(nuevoEquipo.descripcionE);
	printf("\n");
	printf("Ingrese la cantidad de colaboradores que estan en el equipo: ");
	scanf ("%d",&nuevoEquipo.cantidadC);
	registroE = nuevoEquipo.cantidadC;
	for(i=0; i<registroE; i++){
		printf("Ingrese el numero de cedula del colaborador: ");
		scanf("%i", &cedulaE[i]);
	}
	//Prueba de las cedulas de equipo
	for(i=0; i< registroE;i++){
		printf("\n");
		printf("%i", cedulaE[i]);
	}
	printf("\n");
	insertarEquipo(L, nuevoEquipo);
	insertarArista(cedulaE, registroE); //Se insertan las aristas entre los nodos del grafo de colaboradores
}

*Muestra información recolectada*
void mostrarLista(ListaE *L){
	NodoE *i;
	for(i = L->inicio; i!= NULL; i = i->siguiente)
		printf("\n\n-Nombre equipo: %s \n-Descripcion: %s \n-Cantidad: %i ", i->equipo.nombreE, i->equipo.descripcionE, i->equipo.cantidadC); 
	printf("\n");
}

void menuChat(ListaChat *L){
	NodoC *aux;
	NodoC *aux2;
	NodoM *mensaje;
	Arista *auxA;
	Chat nuevoChat;
	int cedulaE, eleccion;
	char destinatario[100];
	char colaboradorR;
	printf("Ingrese su numero de cedula: ");
	scanf("%i",&cedulaE);
	if(inicio==NULL){
		printf("No hay colaboradores registrados en el sistema, intentelo de nuevo\n");
	}
	for(aux=inicio; aux!=NULL; aux=aux->siguiente){
		if(cedulaE==aux->colaborador.cedula){
			printf("1. Enviar un nuevo mensaje \n");
			printf("2. Bandeja de mensajes recibidos\n");
			printf("Seleccione una opcion: ");
			scanf("%i",&eleccion);
			getchar();
			switch(eleccion){
				case 1:
					strcpy(nuevoChat.remitente, aux->colaborador.nombre);
					printf("Ingrese el nombre del destinatario: ");
					gets(destinatario);
					printf("Ingrese el mensaje que desea enviar: ");
					gets(nuevoChat.mensaje);
					auxA=aux->adyacencia;
					for(auxA; auxA!=NULL; auxA=auxA->siguiente){
						if(strcmp(destinatario, auxA->vrt->colaborador.nombre)==0){
							strcpy(nuevoChat.destinatario, auxA->vrt->colaborador.nombre);	
							printf("Mensaje enviado con exito!");
							insertarChat(L,nuevoChat);
							return;
						}
					}
					for(aux2=inicio; aux2!=NULL; aux2=aux2->siguiente){
						if(strcmp(destinatario, aux2->colaborador.nombre)==0){
							strcpy(nuevoChat.destinatario, destinatario);
							//Recorrido, insertar chat 
					}
					}	
				case 2:
					mensaje=L->inicio;
					if(mensaje==NULL){
						printf("No hay ningun mensaje, intentelo de nuevo. ");
						printf("\n");
					}
					for(mensaje; mensaje!=NULL ; mensaje=mensaje->siguiente){
						if(strcmp(aux->colaborador.nombre, mensaje->chat.destinatario )==0){
							printf("\n-Remitente: %s \n-Destinatario: %s \n-Mensaje: %s", mensaje->chat.remitente, mensaje->chat.destinatario, mensaje->chat.mensaje);
					}
					}
					break;
				default:
					printf("\n");
					printf("La opcion seleccionada no es valida, vuelvalo a intentar. ");
					printf("\n");
			}	
			return;
		}
	}
	printf("Numero de cedula no encontrado, intentelo nuevamente. ");
}

ListaBitacora *listaNuevaJ(void)
{
	ListaBitacora *L;
	L = (ListaBitacora *) malloc(sizeof(ListaBitacora));
	L->inicio = NULL;
	L->final = NULL;
	return L;
}

NodoB* crearNodo(Bitacora bitacora)  
{
	NodoB *nuevo;
	nuevo = (NodoB *) malloc(sizeof(NodoB));
	nuevo->anterior = NULL;
	nuevo->siguiente = NULL;
	nuevo->bitacora = bitacora;	 
	
	return nuevo;
}

*Entradas: crear un nuevo nodo que apunte al jugador que se va a insertar*
*Salidas: inserta un jugador al incio de la lista *
*Restricciones: se debe validad que se cree el nodo*

void insertarJugador(ListaBitacora *L, Bitacora bitacora)
{
	if(L->inicio == NULL) //Valida si la lista está vacía
	{
		//Inserta al inicio de la lista
		L->inicio = crearNodo(bitacora);   
		L->final = L->inicio;
		return;
	}
	
	//Inserta el dato al final de la lista, no necesita un ciclo porque tiene el puntero final
	L->final->siguiente = crearNodo(bitacora);  
	L->final->siguiente->anterior = L->final;
	L->final = L->final->siguiente;
}

*Entradas: crear un nuevo nodo que apunte al jugador que se va a insertar*
*Salidas: inserta un jugador al incio de la lista *
*Restricciones: se debe validad que se cree el nodo*

void registrarBitacora(ListaBitacora *L)
{
	Bitacora nuevoJugador;
	NodoB *i = L->inicio;
	printf("Inserte la cedula del colaborador: \n"); 
	if(scanf("%d",&nuevoJugador.cedula)==0){
		for( int c = getchar(); c != EOF && c != ' ' && c != '\n' ; c = getchar());
		printf("\nLa cedula tiene que ser de datos numericos\n\n");
		registrarBitacora(L);
		return;
	}
	getchar();
    printf("\n");
    printf("Ingrese la fecha de hoy: (DD/MM/AAAA) "); gets(nuevoJugador.fecha);  
    printf("\n");
    printf("Inserte el nombre completo del colaborador: "); gets(nuevoJugador.nombreCompleto);  
    printf("\n");
    printf("Ingrese el detalle correspondiente:  "); gets(nuevoJugador.detalle);
   	printf("\n");
    insertarJugador(L,nuevoJugador);
	printf("\nDatos del jugador %s guardados\n", nuevoJugador.nombreCompleto)
}

*Entradas: la lista con los jugadores*
*Salidas: crear un archivo que contenga la información de los jugadores*
*Restricciones: se debe validar que se hayn guardado los jugadores correctamente en el archivo*

void guardarBitacora(ListaBitacora *L){
	printf("\n\n\nGuardando la informacion de los colaboradores en la bitácora");
	FILE * archivo = fopen("Bitacora.dat", "wb");
	if(archivo == NULL){
		printf("\n\n\nError al guardar en archivo");
		return;
	}
	NodoB * curr = L->inicio;
	while (curr != NULL){
		fwrite(&(curr->bitacora), sizeof(Bitacora), 1, archivo); 
		curr = curr->siguiente;
		}
	fclose(archivo);
}

*Entradas: la lista con la información de los jugaodres*
*Salidas: cargar el archivo con la información de los jugadores*
*Restricciones: se debe verficar que se ha cargado correctamente el archivo*

void cargarBitacora(ListaBitacora *L){
	FILE * archivo = fopen("Bitacora.dat", "rb");
	if(archivo == NULL){
		printf("\n\n\nError al cargar desde archivo");
		return;
	}
	Bitacora bitacora; 
	int leidos;
	do{
		leidos = fread(&(bitacora), sizeof(Bitacora), 1, archivo);
		if (leidos == 1)
			insertarJugador(L, bitacora);
	} while (leidos == 1);
 	printf("\nSe cargaron los detalles de la bitácora en el archivo\n");
	fclose(archivo);
}

*Entradas: cedula ingresada, funciona como identificador unico*
*Salidas: información del usuario o jugador*
*Restricciones: cedula debe existir en la lista*

void consultarBitacora(ListaBitacora *L)
{
	int eleccion, cedula;
	NodoB *i;
	if (L== NULL){
		printf("No hay registros de dicho colaborador");
	}
	for(i = L->inicio; i!= NULL; i = i->siguiente)
		printf("\n\n-Nombre completo: %s  \n-Cedula: %i ", i->bitacora.nombreCompleto, i->bitacora.cedula);  
	i= L->inicio;
	printf("\n");
	printf ("Digite el numero de cedula del colaborador a consultar: ");
	if (scanf("%d", &cedula)==0){
		for( int c = getchar(); c != EOF && c != ' ' && c != '\n' ; c = getchar());
			cedula=01020162;
	}
	for(i; i != NULL; i = i->siguiente){
		if (cedula == i->bitacora.cedula){
			printf("\n \nFecha: %s \n-Nombre completo: %s \n-Cedula: %d \n-Detalle: %s", i->bitacora.fecha, i->bitacora.nombreCompleto, i->bitacora.cedula, i->bitacora.detalle);   ///cambio, aqui solo estoy imprimiendo el nombre de cada jugador
			printf("\n");
			return;
		}
	}
	printf("\nNo hay registros de dicho colaborador en la bitácora\n");
}


void menuPrincipal(){
	int eleccion;
	ListaE *L;
	L= listaNuevaE();
	system("cls");
	while(eleccion != 12){
		printf("\n---------------------MENU-----------------------\n");
		printf("\n 1. Registrar Colaborador");
		printf("\n 2. Registrar Equipo");
		printf("\n 3. Mostrar Equipos (temporal)");
		printf("\n 4. Mostrar grafo de colaboradores (temporal)");
		printf("\n 5. Ingresar/Actualizar Bitácora");
		printf("\n 6. Ver Bitácora");
		printf("\n 7. Aux");
		printf("\n 8. Aux");
		printf("\n 9. Aux");
		printf("\n 10. Aux");
		printf("\n 11. Aux");
		printf("\n 12. Salir");
		putchar('\n');
		printf("\n------------------------------------------------\n");
		printf("\n Seleccione una opcion:");
		if (scanf ("%d",&eleccion) == 0){
  			for( int c = getchar(); c != EOF && c != ' ' && c != '\n' ; c = getchar());
			eleccion = 0;
		}		
		switch(eleccion){
			case 1:
				registrarColaborador();
				break;
			case 2:
				registrarEquipo(L);
				break;
			case 3:
				mostrarLista(L);
				break;
			case 4:
				visualizarGrafo();
				break;
			case 5:
				registrarBitacora(L2);
				break;
			case 6:
				consultarBitacora(L2);
				break;
			case 7:
				//////////
				break;
			case 8:
				///////////
				break;
			case 9:
				/////////
				break;
			case 10:
				//////////
				break;
			case 11:
				//////////
				break;
			case 12:
				printf("\nCerrando sistema.....\n");
				break;
			default:
				putchar('\n');
				printf("\nValor invalido, por favor ingrese una de las siguientes opciones: \n");
		}
		system("pause");
        system("cls");
	}
}
int main(){
	menuPrincipal();
}
