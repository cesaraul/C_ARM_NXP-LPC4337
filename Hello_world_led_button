

/*
 * Date: 2019-02-13
 * ing. Cesar Cruz
 * Código desarrollado haciendo uso de la libreria Sapi
 * desarrollo en baremetal de una maquina de estados para un antirebote
 * implementado en un ARM Cortex M4 NXP LPC4337, placa eduCIAA
 */



#include "sapi.h"              // <= sAPI header

typedef enum{       // Estados para antirebote
	BUTTON_UP,
	BUTTON_DOWN,
	BUTTON_FALLING,
	BUTTON_RISING

}fsmButtonState_t;

void fsmButtonError(void);   //error
void fsmButtonInit(void);    //estado inicial
void fsmButtonUpdate(gpioMap_t tecla); // maquina de estados para actualización
void buttonPressed(void);  	 //funcion para acciones en estado de transicion buttonpressed
void buttonReleased(void);   //funcion para acciones en estado de transicion Button Reased

fsmButtonState_t fsmButtonState; //NUEVA variable fsmbuttonstate



/* FUNCION PRINCIPAL, PUNTO DE ENTRADA AL PROGRAMA LUEGO DE RESET. */
void buttonPressed(void)  // acciones en función de transición de estado
{
	}
void buttonReleased(void){  //acciones para función de transición -- cambio de estado de LED

		    	  gpioToggle(LED3);


}
void fsmButtonError(void) // detección de error retorna a estado UP
{
	fsmButtonState=BUTTON_UP;

	}
void fsmButtonInit(void) // funciñon para inicializar en estado normal button UP
{
	fsmButtonState = BUTTON_UP;
	}
void fsmButtonUpdate(gpioMap_t tecla)  // maquina de estados , actualización
{
	static bool_t flagFalling = FALSE;   //
	static bool_t flagRising = FALSE;    //

	static uint8_t contFalling = 0;      //
	static uint8_t contRising =0;       //

	switch (fsmButtonState)   // switch de maquina de estados
	{
	case BUTTON_UP:
		if(!gpioRead(tecla)) // botones estan a pullup se activa la condición al detectar un down
		{
			fsmButtonState = BUTTON_FALLING;  //CAMBIO DE ESTADO

			}
		break;
	case BUTTON_DOWN: //
		if(gpioRead(tecla)){ //Estado espera a que el boton vuelva a 1
			fsmButtonState = BUTTON_RISING; // pasa a evaluar si el boton se precionó

			}
		break;
	case BUTTON_FALLING:
		if(flagFalling == FALSE){ // FLAG inicializado en FALSE
			flagFalling=TRUE;
			//gpioWrite(LED1,ON);
			}
		if(contFalling>=40){ //40 ms de espera para confirmar boton presionado
			if(!gpioRead(tecla)){//si el boton es presionado , cambio de estado y activa función Pressed
				fsmButtonState = BUTTON_DOWN;
				buttonPressed(); //UP TO DOWN ACTIVADO
				}else{
					fsmButtonState = BUTTON_UP;//si el boton no fue precionado vuelve a estado incial
				}
			contFalling =0; // se reinicia contador en cualquier caso
			}
		contFalling++;  // este es el contador que nos ayuda a estimar el tiempo

		if(fsmButtonState != BUTTON_FALLING){  // desactivación de flag si existe cambio de estado
			flagFalling = FALSE;
			//gpioWrite(LED1,OFF);
			}
		break;

	case BUTTON_RISING:

		if(flagRising == FALSE){  // activando flag
			flagRising = TRUE;
			//gpioWrite(LED2,ON);
			}
		if (contRising>=40)// verificar gpio despues de 40mili segundos
		{
			if(gpioRead(tecla)){
				fsmButtonState = BUTTON_UP;
				buttonReleased(); // DOW TO UP ACTIVADO , funcion de acciones en estado de transición
			} else{

				fsmButtonState = BUTTON_DOWN;// indica que el boton se mantiene presionado
			}
			contRising = 0;

			}
		contRising++; //contador para los 40 ms

		if (fsmButtonState !=BUTTON_RISING){

			flagRising=FALSE; //ACTUALIZAR ESTADO DE FLAG
			//gpioWrite(LED2,OFF);
			}
		break;

		default:
			fsmButtonError();
		break;
		}
	}
int main(void){


   boardConfig(); // inicializacion de placa , Leds Botones , libreria SAPI

   fsmButtonInit(); // inicialización de anti rebote

   delay_t delay; // inicialización de retardo no bloqueante

   delayConfig( &delay, 500 ); // set delay (500ms = 0,5s)


   /* ------------- REPETIR POR SIEMPRE ------------- */
   while(1) {    // LOOP

     // fsmButtonUpdate(TEC2);
     // fsmButtonUpdate(TEC3);
      fsmButtonUpdate(TEC4);  // update para un boton


   }

   /* NO DEBE LLEGAR NUNCA AQUI, debido a que a este programa no es llamado
      por ningun S.O. */
   return 0 ;
}

/*==================[end of file]============================================*/
