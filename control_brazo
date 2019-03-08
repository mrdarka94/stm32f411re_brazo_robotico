#include "mbed.h"
 
/*****************************************************************************
Este archivo es una modificacion al ejercicio propuesta por el ingeniero Ferney alberto Beltran
molina. 
generar un programa que controle por el puerto serial el grado de 4 servo motores.
por medio de la comunicacion serial el comando es 
 
|            |                 |             |             |               |
|   INITCMD  |   Tipo_Comando  |   N_grados  | Parametro_1 | Funcion       |
|     0xff   |        0x01     |     0x00    |    0x01     | Pos. Home     |
|     0xff   |        0x01     |     0x00    |    0x02     | Pos. Material |
|     0xff   |        0x01     |     0x00    |    0x03     | Pos. Celda 1  |
|     0xff   |        0x01     |     0x00    |    0x04     | Pos. Celda 2  |
|     0xff   |        0x01     |     0x00    |    0x05     | Pos. Celda 3  |
|     0xff   |        0x01     |     0x00    |    0x06     | Pos. Celda 4  |
|     0xff   |        0x02     |     0x00    |    0x01     | Abre Pinza    |
|     0xff   |        0x02     |     0x00    |    0x02     | Cierra Pinza  |
|     0xff   |        0x03     | 0x00 - 0xb4 |    0x00     | Mueve Motor 1 |
|     0xff   |        0x04     | 0x00 - 0xb4 |    0x00     | Mueve Motor 2 |
|     0xff   |        0x05     | 0x00 - 0xb4 |    0x00     | Mueve Motor 3 |
|     0xff   |        0x06     | 0x00 - 0xb4 |    0x00     | Mueve Motor 4 |
 
para enviar los comandos usar el programa Coolterm http://freeware.the-meiers.org/
 
# para el servo motor se debe modificar el ciclo util del PWM SEGUN:
#     ________              ________
# ___|        |____________|        |___________
#    <-width->
#    <-------period 20ms--->
# period = 20 ms
# width  = 1000us y 2000us
#  1000us para 0 grados
#  2000us para 180 grados
#
 
*****************************************************************************/
 
// configuracion comunicación serial 
Serial command(USBTX, USBRX);
// seleccion de los pines a usar en la stm32f411re
PwmOut myservo1(PB_4);
PwmOut myservo2(PB_3);
PwmOut myservo3(PB_10);
PwmOut myservo4(PC_7);

 
#define INITCMD 0xFF
#define DEGREES_MAX 180
 
// definición de las variables globales 
 

uint8_t N_grados;    // varable almacena los grados que se mueve el motor en leer_datos()
uint8_t Tipo_Comando; // varable que almacena el numero de seleccion de comando
uint8_t Parametro_1; // varable que almacena el parametro a ejecutar
 
// definición de las funciones
void setup_uart();
void setup_servo();
 
void mover_servo(uint8_t T_C, uint8_t grados, uint8_t Par_1);
uint32_t degrees2usec(uint8_t grados);
 
void leer_datos();
 
    
    
int main() {
 
    setup_uart();
    setup_servo();
    command.printf("inicio de programa");
    while(1){    
        leer_datos();
        mover_servo(Tipo_Comando, N_grados, Parametro_1);
    }    
}
 
 
 
void setup_uart(){
    command.baud(115200);
}
 
void setup_servo(){
    myservo1.period_ms(20);
    myservo1.pulsewidth_us(700);
    myservo2.period_ms(20);
    myservo2.pulsewidth_us(700);
    myservo3.period_ms(20);
    myservo3.pulsewidth_us(700);
    myservo4.period_ms(20);
    myservo4.pulsewidth_us(700);
}
// funcion leer_datos obtiene los comandos enviados desde cool Terminal 
void leer_datos(){
    while(command.getc()!= INITCMD);
    Tipo_Comando=command.getc();
    N_grados=command.getc();
    Parametro_1=command.getc();
    
}
 
 
uint32_t degrees2usec(uint8_t grados){
// Retorna el valor en microsegundos, donde
//      y − y1 = m(x − x1 )
//      y=  b + mx
 
    if(grados <= DEGREES_MAX)
        return int(750+grados*1800/180);// u6
    return 750;
    
}
 
void mover_servo(uint8_t T_C, uint8_t grados, uint8_t Par_1){
 
        uint32_t dpulse=55;
        
 
                
    dpulse=degrees2usec(grados);
    
    switch(T_C){
        case 1:
                if (Par_1==1){//posicion home
                     myservo2.pulsewidth_us(degrees2usec(90));
                     wait(2);
                     myservo1.pulsewidth_us(degrees2usec(5));
                     wait(1);
                     myservo2.pulsewidth_us(degrees2usec(70));
                     wait(1);
                     myservo3.pulsewidth_us(degrees2usec(90));
                     //myservo4.pulsewidth_us(degrees2usec(0));

                     }
                if (Par_1==2){//posicion material
                     myservo2.pulsewidth_us(degrees2usec(90));
                     wait(2);
                     myservo1.pulsewidth_us(degrees2usec(170));
                     wait(1);
                     myservo2.pulsewidth_us(degrees2usec(90));
                     wait(1);
                     myservo3.pulsewidth_us(degrees2usec(70));
                     //myservo4.pulsewidth_us(degrees2usec(45));     
                    }
                if (Par_1==3){//posicion celda 1
                     myservo2.pulsewidth_us(degrees2usec(90));
                     wait(2);
                     myservo1.pulsewidth_us(degrees2usec(140));
                     wait(1);
                     myservo2.pulsewidth_us(degrees2usec(110));
                     wait(1);
                     myservo3.pulsewidth_us(degrees2usec(grados));
                     //myservo4.pulsewidth_us(degrees2usec(grados));
                     }
                if (Par_1==4){//posicion celda 2
                     myservo2.pulsewidth_us(degrees2usec(90));
                     wait(2);
                     myservo1.pulsewidth_us(degrees2usec(110));
                     wait(1);
                     myservo2.pulsewidth_us(degrees2usec(110));
                     wait(1);
                     myservo3.pulsewidth_us(degrees2usec(grados));
                     //myservo4.pulsewidth_us(degrees2usec(grados));
                     }
                if (Par_1==5){//posicion celda 3
                     myservo2.pulsewidth_us(degrees2usec(90));
                     wait(2);
                     myservo1.pulsewidth_us(degrees2usec(80));
                     wait(1);
                     myservo2.pulsewidth_us(degrees2usec(110));
                     wait(1);
                     myservo3.pulsewidth_us(degrees2usec(grados));
                     wait(1);
                     //myservo4.pulsewidth_us(degrees2usec(grados));
                     }
                if (Par_1==6){//posicion celda 4
                     myservo2.pulsewidth_us(degrees2usec(90));
                     wait(2);
                     myservo1.pulsewidth_us(degrees2usec(50));
                     wait(1);
                     myservo2.pulsewidth_us(degrees2usec(110));
                     wait(1);
                     myservo3.pulsewidth_us(degrees2usec(grados));
                     //myservo4.pulsewidth_us(degrees2usec(grados));
                     }
            break;
                
        case 2:
                if (Par_1==1){//abrir pinza
                     //myservo4.pulsewidth_us(degrees2usec(grados));
                     }
                     
                if (Par_1==2){//cerrar pinza
                     //myservo4.pulsewidth_us(degrees2usec(grados));
                     }  
            break;
               
        case 3:
                 myservo1.pulsewidth_us(dpulse);
            break;
                 
        case 4:
                if(grados>=55){
               myservo2.pulsewidth_us(dpulse);}
            break;
        case 5:
            if(grados>=55){
               myservo3.pulsewidth_us(dpulse);}
            break;
        case 6:
                if(grados>=55)
               myservo4.pulsewidth_us(dpulse);  
            break;

        
                     
    }
 
} 
