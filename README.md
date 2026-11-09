/*
 * GUARDIANES DEL AGUA: DUELO EN GDL
 * Proyecto de Programacion Estructurada - CETI
 * Integrantes: Efrain Wlises Gonzalez Sandoval,
 *              Juan Alejandro Ramirez Ayon,
 *              Cesar Octavio Raygoza Martinez
 *
 * Compilar:  gcc guardianes_del_agua.c -o guardianes
 * Ejecutar:  ./guardianes
 */

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

#define NUM_PERS  10
#define NUM_HAB   8
#define NUM_DATOS 5
#define RONDAS    3

/* ---------- ESTRUCTURAS ---------- */

typedef struct {
    char nombre[40];
    int ingenio, conciencia, energia;
    char frase[100];      /* dato educativo para la galeria */
} Personaje;

typedef struct {
    char nombre[30];
    int bono;
} Habilidad;

/* ---------- DATOS GLOBALES ---------- */

Personaje personajes[NUM_PERS] = {
    {"Dona Chela, La Aguadora",       7, 9, 5, "Cerrar la llave al enjabonarte ahorra hasta 10 litros."},
    {"Gota, el Espiritu de Chapala",  6, 7, 9, "El lago de Chapala es la mayor fuente de agua de Jalisco."},
    {"Ing. Tubo",                     9, 6, 6, "Una fuga pequena puede desperdiciar cientos de litros al mes."},
    {"Capitan Cisterna",              6, 7, 8, "Almacenar agua de lluvia ayuda en temporada de secas."},
    {"La Lluvia Tapatia",             7, 8, 7, "En Guadalajara las lluvias fuertes llegan de junio a septiembre."},
    {"Profe Filtro",                  8, 8, 5, "Filtrar y hervir el agua la hace segura para beber."},
    {"Neco-Aqua",                     9, 5, 7, "Un gato travieso tambien sabe que el agua no se tira."},
    {"Don Regadera",                  6, 9, 6, "Banos de 5 minutos ahorran mucha agua cada semana."},
    {"Nina Presa",                    7, 7, 8, "Las presas guardan el agua que usamos en temporada de secas."},
    {"Maestro Rio",                   8, 8, 6, "El rio Santiago nace en el Lago de Chapala y cruza Jalisco."}
};

Habilidad habilidades[NUM_HAB] = {
    {"Cubetazo",               3},
    {"Cierra la llave",        4},
    {"Detecta fuga",           5},
    {"Riego inteligente",      3},
    {"Reutiliza el agua",      4},
    {"Captura de lluvia",      5},
    {"Regadera ahorradora",    2},
    {"Cuida la cuenca",        6}
};

const char *datosEducativos[NUM_DATOS] = {
    "Una llave goteando puede desperdiciar litros de agua al dia.",
    "Regar las plantas de noche evita que el agua se evapore rapido.",
    "Reutilizar el agua de la lavadora sirve para trapear o regar.",
    "Un excusado con fuga puede gastar mas de 100 litros al dia.",
    "Solo una pequena parte del agua del planeta es dulce y utilizable."
};

/* ---------- DIBUJOS ASCII ---------- */

void mostrarDibujo(int id) {
    switch (id) {
        case 0:
            puts("     .-~~-.");
            puts("    ( o  o )");
            puts("    (  <>  )");
            puts("   __|----|__");
            puts("  (  | OLLA|  )");
            puts("   '-|_____|-'");
            break;
        case 1:
            puts("        .");
            puts("       /:)");
            puts("      / : )");
            puts("     / o o )");
            puts("    (  ___  )");
            puts("     '-___-'");
            break;
        case 2:
            puts("     .---.");
            puts("    /_____)");
            puts("    ( o o )");
            puts("    (  ^  )");
            puts("  <=|-----|");
            puts("    |_____|");
            puts("     |   |");
            break;
        case 3:
            puts("      _/^)_");
            puts("     (_____)");
            puts("     ( o o )");
            puts("   .-|=====|-.");
            puts("   | | H2O | |");
            puts("   '-|_____|-'");
            break;
        case 4:
            puts("      .--.");
            puts("   .-( o o ).");
            puts("  (___.--.___)");
            puts("    ' ' ' ' '");
            puts("     ' ' ' '");
            puts("    ' ' ' ' '");
            break;
        case 5:
            puts("   .---------.");
            puts("   | [o] [o] |");
            puts("   '.  ___  .'");
            puts("     '.   .'");
            puts("      |   |");
            puts("      | : |");
            puts("       'v'");
            break;
        case 6:
            puts("    ^~~~^");
            puts("   (=^.^=)");
            puts("    ( ~ )");
            puts("   ~(_|_)~");
            puts("  ~~~~~~~~~");
            break;
        case 7:
            puts("    .-----.");
            puts("   ( o   o )====,");
            puts("    '-----'   ' ' '");
            puts("    |  -  |    ' '");
            puts("    '-----'");
            break;
        case 8:
            puts("     ,-^-.");
            puts("    ( o o )");
            puts("    ( ' ' )~");
            puts("   |||||||||");
            puts("   |||~~~~||");
            puts("   |||~~~~||");
            break;
        case 9:
            puts("     .-^-.");
            puts("    ( o o )");
            puts("    ( === )");
            puts("    ~~~~~~~~");
            puts("   ~~~~~~~~~~");
            puts("    ~~~~~~~~");
            break;
    }
}

/* ---------- LAS 4 FUNCIONES REQUERIDAS ---------- */

/* 1) No retorna y no recibe */
void mostrarPortada() {
    puts("==========================================");
    puts("   GUARDIANES DEL AGUA: DUELO EN GDL");
    puts("   Creado por: Efrain Wlises Gonzalez Sandoval,");
    puts("               Juan Alejandro Ramirez Ayon,");
    puts("               Cesar Octavio Raygoza Martinez");
    puts("==========================================");
}

/* 2) Retorna y no recibe: lee la opcion del usuario */
int leerOpcion() {
    char linea[50];
    int opcion;
    printf("> ");
    if (fgets(linea, sizeof(linea), stdin) == NULL) {
        return 0;                       /* fin de entrada: salir */
    }
    if (sscanf(linea, "%d", &opcion) != 1) {
        return -1;                      /* entrada no valida */
    }
    return opcion;
}

/* 3) Recibe y no retorna */
void mostrarPersonaje(Personaje p) {
    printf("%s\n", p.nombre);
    printf("   Ingenio: %d | Conciencia: %d | Energia: %d\n",
           p.ingenio, p.conciencia, p.energia);
    printf("   \"%s\"\n", p.frase);
}

/* 4) Retorna y recibe: puntaje de la ronda */
int calcularPuntaje(Personaje p, int h1, int h2, int h3) {
    int base  = p.ingenio + p.conciencia + p.energia;
    int bonos = habilidades[h1].bono + habilidades[h2].bono + habilidades[h3].bono;
    int azar  = rand() % 6;             /* valor aleatorio de 0 a 5 */
    return base + bonos + azar;
}

/* ---------- FUNCIONES AUXILIARES ---------- */

void mostrarMenu() {
    puts("\n------------- MENU PRINCIPAL -------------");
    puts(" 1. Personajes (galeria)");
    puts(" 2. Instrucciones");
    puts(" 3. Jugar");
    puts(" 4. Salir");
    puts("------------------------------------------");
}

void mostrarGaleria() {
    int i;
    puts("\n======== GALERIA DE GUARDIANES ========");
    for (i = 0; i < NUM_PERS; i++) {
        printf("\n%d) ", i + 1);
        mostrarPersonaje(personajes[i]);
        mostrarDibujo(i);
    }
}

void mostrarInstrucciones() {
    puts("\n============ INSTRUCCIONES ============");
    puts("1. Elige \"Jugar\" en el menu principal.");
    puts("2. En cada ronda escoge un guardian por su numero");
    puts("   (no puedes repetir guardian en las 3 rondas).");
    puts("3. El juego le sortea 3 habilidades al azar a tu guardian,");
    puts("   y la computadora hace lo mismo con el suyo.");
    puts("4. Puntaje = ingenio + conciencia + energia + bonos de");
    puts("   habilidades + un pequeno valor aleatorio.");
    puts("5. Gana la ronda quien tenga mas puntos.");
    puts("6. Despues de 3 rondas se corona al campeon. Si hay empate,");
    puts("   gana quien haya sumado mas puntos en total.");
}

/* Sortea 3 habilidades distintas */
void sortearHabilidades(int h[3]) {
    h[0] = rand() % NUM_HAB;
    do { h[1] = rand() % NUM_HAB; } while (h[1] == h[0]);
    do { h[2] = rand() % NUM_HAB; } while (h[2] == h[0] || h[2] == h[1]);
}

void mostrarHabilidades(int h[3]) {
    int i;
    for (i = 0; i < 3; i++) {
        printf("   - %s (+%d)\n", habilidades[h[i]].nombre, habilidades[h[i]].bono);
    }
}

/* Pide al jugador un personaje que no haya usado */
int elegirPersonaje(int usados[]) {
    int i, op;
    puts("\nGuardianes disponibles:");
    for (i = 0; i < NUM_PERS; i++) {
        if (!usados[i]) {
            printf(" %2d. %s\n", i + 1, personajes[i].nombre);
        }
    }
    do {
        puts("Elige el numero de tu guardian:");
        op = leerOpcion();
        if (op < 1 || op > NUM_PERS || usados[op - 1]) {
            puts("Opcion no valida o ya usada, intenta de nuevo.");
            op = -1;
        }
    } while (op == -1);
    return op - 1;
}

/* La computadora elige al azar uno que no haya usado */
int elegirPersonajeCPU(int usados[]) {
    int id;
    do { id = rand() % NUM_PERS; } while (usados[id]);
    return id;
}

void jugar() {
    int usadosJ[NUM_PERS] = {0};
    int usadosC[NUM_PERS] = {0};
    int ronda, idJ, idC;
    int hJ[3], hC[3];
    int ptsJ, ptsC;
    int ganadasJ = 0, ganadasC = 0;
    int totalJ = 0, totalC = 0;

    for (ronda = 1; ronda <= RONDAS; ronda++) {
        printf("\n=============== RONDA %d de %d ===============", ronda, RONDAS);

        idJ = elegirPersonaje(usadosJ);
        usadosJ[idJ] = 1;
        idC = elegirPersonajeCPU(usadosC);
        usadosC[idC] = 1;

        sortearHabilidades(hJ);
        sortearHabilidades(hC);

        printf("\nTU GUARDIAN:\n");
        mostrarDibujo(idJ);
        mostrarPersonaje(personajes[idJ]);
        puts("   Habilidades sorteadas:");
        mostrarHabilidades(hJ);

        printf("\nGUARDIAN RIVAL:\n");
        mostrarDibujo(idC);
        mostrarPersonaje(personajes[idC]);
        puts("   Habilidades sorteadas:");
        mostrarHabilidades(hC);

        ptsJ = calcularPuntaje(personajes[idJ], hJ[0], hJ[1], hJ[2]);
        ptsC = calcularPuntaje(personajes[idC], hC[0], hC[1], hC[2]);
        totalJ += ptsJ;
        totalC += ptsC;

        printf("\nPuntaje -> Tu: %d | Rival: %d\n", ptsJ, ptsC);
        if (ptsJ > ptsC) {
            puts(">> Ganaste la ronda!");
            ganadasJ++;
        } else if (ptsC > ptsJ) {
            puts(">> La computadora gana la ronda.");
            ganadasC++;
        } else {
            puts(">> Empate en la ronda.");
        }
        printf("\nDato educativo: %s\n", datosEducativos[rand() % NUM_DATOS]);
    }

    puts("\n================ RESULTADO FINAL ================");
    printf("Rondas ganadas -> Tu: %d | Rival: %d\n", ganadasJ, ganadasC);
    printf("Puntaje total  -> Tu: %d | Rival: %d\n", totalJ, totalC);
    if (ganadasJ > ganadasC || (ganadasJ == ganadasC && totalJ > totalC)) {
        puts("*** FELICIDADES! ERES EL GUARDIAN CAMPEON DEL AGUA ***");
    } else if (ganadasC > ganadasJ || (ganadasJ == ganadasC && totalC > totalJ)) {
        puts("*** La computadora es la campeona. Gracias por cuidar el agua! ***");
    } else {
        puts("*** Empate total. Todos ganamos si cuidamos el agua! ***");
    }
}

/* ---------- PROGRAMA PRINCIPAL ---------- */

int main() {
    int opcion;
    srand((unsigned) time(NULL));

    mostrarPortada();

    do {
        mostrarMenu();
        opcion = leerOpcion();
        switch (opcion) {
            case 1: mostrarGaleria();       break;
            case 2: mostrarInstrucciones(); break;
            case 3: jugar();                break;
            case 4:
            case 0: puts("\nGracias por jugar. Cuida el agua!"); opcion = 4; break;
            default: puts("Opcion no valida.");
        }
    } while (opcion != 4);

    return 0;
}
