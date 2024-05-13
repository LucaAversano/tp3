<a>  1_ a. el conhilos tarda menos que el sinhilos. No es predecible
        b. en el sinhilos si pero el de conhilos no
        c. tira un error cuando descomentas. Porque se espera un bloque de sangria despues del para

    2_ a.#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>

#define NUMBER_OF_THREADS 2
#define CANTIDAD_INICIAL_HAMBURGUESAS 20

int cantidad_restante_hamburguesas = CANTIDAD_INICIAL_HAMBURGUESAS;
int turno = 0; // Variable para el mecanismo de turno

void *comer_hamburguesa(void *tid)
{
    while (1 == 1)
    {
        while (turno != (int)tid); // Espera su turno
        
        // INICIO DE LA ZONA CRÍTICA
        if (cantidad_restante_hamburguesas > 0)
        {
            printf("Hola! Soy el hilo (comensal) %d. Me voy a comer una hamburguesa, todavía quedan %d.\n", (int)tid, cantidad_restante_hamburguesas);
            cantidad_restante_hamburguesas--; // Se come una hamburguesa
        }
        else
        {
            printf("¡SE TERMINARON LAS HAMBURGUESAS! :( \n");
            pthread_exit(NULL); // Forzar terminación del hilo
        }
        // FIN DE LA ZONA CRÍTICA

        turno = (turno + 1) % NUMBER_OF_THREADS; // Cambia el turno
    }
}

int main(int argc, char *argv[])
{
    pthread_t threads[NUMBER_OF_THREADS];
    int status, i, ret;

    for (i = 0; i < NUMBER_OF_THREADS; i++)
    {
        printf("Hola, soy el hilo principal. Estoy creando el hilo %d\n", i);
        status = pthread_create(&threads[i], NULL, comer_hamburguesa, (void *)i);
        if (status != 0)
        {
            printf("Algo salió mal al crear el hilo. Código de error: %d\n", status);
            exit(-1);
        }
    }

    for (i = 0; i < NUMBER_OF_THREADS; i++)
    {
        void *retval;
        ret = pthread_join(threads[i], &retval); // Espero la terminación de los hilos creados
    }
    
    pthread_exit(NULL); // Como los hilos ya terminaron, termino yo también
}

b. <img scr= "dosb.jpg">
</a>