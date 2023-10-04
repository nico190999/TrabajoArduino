# Trabajo grupal TinkerCad 
![Tinkercad]()


## Integrantes 
- Pedro Serra
- Nicolas Sanchez 
- Mateo Roberto


## Proyecto: Multiplexación.
![Tinkercad]()


## Descripción
Consta de una placa de Arduino, de la cual se desprenden diversos cables que otorgan funcionamiento a través de una diferencia de potencial a 2 display de 7 segmentos y 3 botones. Los tres botones realizan modificaciones en el numero que forman ambos displays.

## Función principal
A traves de los botones del proyecto permite incrementar, reducir o resetear el número que forman ambos display, este va desde 00 hasta 99. Cada uno de los botones tiene su función de manera independiente. En caso que, se incremente en 1 valor el número que se muestra en los display cuando este se encuentra en "99", volvera a "00"; y si se reduce en 1 el valor del display cuando este se encuentre en "00", volvera a "99".



~~~ C (lenguaje en el que esta escrito)
/*
Defino diferentes nombres a los pines con los que 
voy a trabajar
*/

#define A 12
#define B 13
#define C 7
#define D 8
#define E 9
#define F 11
#define G 10
#define SUBE 4
#define BAJA 3
#define RESET 5
#define UNIDAD A4
#define DECENA A5
#define APAGADOS 0
#define TIEMPO 10


//******************************

/*
Seteo variables con sus respectivos valores en 1(uno)
Seteo un contador en 0(cero)
*/

int sube = 1;
int subePrevia = 1;
int baja = 1;
int bajaPrevia = 1;
int reset = 1;
int resetPrevia = 1;

int contador = 0;

//******************************

/* 
- Configuro los pines
	Entrada: 3, 4 y 5
	Salida: 7, 8, 9, 10, 11, 12, 13, A4 y A5
(digitalWrite creo que lo puedo sacar)
- Llamo a la funcion mostrarNumero para setear los displays en 0
*/

void setup()
{
    Serial.begin(9600);
  	pinMode(3, INPUT_PULLUP);
    pinMode(4, INPUT_PULLUP);
    pinMode(5, INPUT_PULLUP);
    pinMode(7, OUTPUT);
    pinMode(8, OUTPUT);
    pinMode(9, OUTPUT);
    pinMode(10, OUTPUT);
    pinMode(11, OUTPUT);
    pinMode(12, OUTPUT);
    pinMode(13, OUTPUT);
  	pinMode(A4, OUTPUT);
  	pinMode(A5, OUTPUT);
  	digitalWrite(UNIDAD, LOW);
  	digitalWrite(DECENA, LOW);
  
}


//******************************



void loop()
{
  /*
  Se llama a la funcion keyPressed(), y se le asigna el valor a
  pressed, que puede ser
  	-SUBE
    -BAJA
    -RESET
  */
  
  /*
  Segun este valor asignado, aumenta, disminuye o resetea el 
  valor del contador
  */
  
  	int pressed = keyPressed();
  	if(pressed == SUBE)
    {
      contador++;
      if(contador > 99)
      contador = 0;
    }
    else if(pressed == BAJA)
    {
      contador--;
      if(contador < 0)
      contador = 99;
    }
  else if(pressed == RESET)
  {
   contador = 0;
  }
  
  /*
  Llamo a la funcion printCount() con el valor
  del contador
  */
  
  printCount(contador);
}

//*********************************

int keyPressed(void)
{
  /*
  Se setean las variables sube, baja y reset
  Si no se oprimen los botones se leen como 1(uno)
  cuando se opriman los botones se leera 0(cero)
  */
  
  	sube = digitalRead(SUBE);
  	baja = digitalRead(BAJA);
  	reset = digitalRead(RESET);	
  
  /*
  En cualquier momento que se desprima algun boton se van a 
  igualar los valores de valorPrevio y valor actual. Por ello
  no van a poder entrar al loop donde se retorna respuesta
  */
  	
  	if (sube == 1)
      subePrevia = 1;
  	if (baja)
      bajaPrevia = 1;
  	if (reset)
      resetPrevia = 1;
  
  /*
  En la primera vuelta de loop que se oprime el boton, el valor
  del actual sera 0 y el valorPrevio sera 1, y asi entra en el 
  condicional if y retornar respuesta
  */
  
  /*
  En la primera vuelta de loop, se le asigna al valorPrevio el 
  valor del valor actual. De este modo no vuelve a entrar
  en el loop
  */
  
  	if(sube == 0 && sube != subePrevia)
    {
    	subePrevia = sube;
      Serial.println(UNIDAD);
      	return SUBE;
    }
  	
  	if(baja == 0 && baja != bajaPrevia)
    {
    	bajaPrevia = baja;
        return BAJA;
    }
  
  	if(reset == 0 && reset != resetPrevia)
    {
    	resetPrevia = reset;
      	return RESET;
    }
  return 0;
}

//*********************************

void apagarDisplay()
{
  /*
  Funcion para apagar todos los leds
  */
    digitalWrite(A, LOW);
    digitalWrite(B, LOW);
    digitalWrite(C, LOW);
    digitalWrite(D, LOW);
    digitalWrite(E, LOW);
    digitalWrite(F, LOW);
    digitalWrite(G, LOW);
}

void mostrarNumero(int numero)
{
  /*
  Primero apago todos los leds
  Luego prendo los leds correspondientes al numero que
  ingresa como parametro
  */
    apagarDisplay();
    switch (numero)
    {
    case 0:
        digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(E, HIGH);
        digitalWrite(F, HIGH);
        break;
    case 1:
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        break;
    case 2:
        digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(E, HIGH);
        digitalWrite(G, HIGH);
        break;
    case 3:
        digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(G, HIGH);
        break;
    case 4:
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(F, HIGH);
        digitalWrite(G, HIGH);
        break;
    case 5:
        digitalWrite(A, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(F, HIGH);
        digitalWrite(G, HIGH);
        break;
    case 6:
        digitalWrite(A, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(E, HIGH);
        digitalWrite(F, HIGH);
        digitalWrite(G, HIGH);
        break;
    case 7:
        digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        break;
    case 8:
        digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(E, HIGH);
        digitalWrite(F, HIGH);
        digitalWrite(G, HIGH);
        break;
    case 9:
        digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(F, HIGH);
        digitalWrite(G, HIGH);
        break;
    }
}

//*********************************

void prendeDigito(int digito)
{
  /*
  Con esta funcion puedo determinar que display se
  prende, en relacion a que digito ingresa como parametro
  Si ingresa UNIDAD, 
  	-prendo display de derecha y apago el de la izquierda
  Si ingresa DECENA
  	-prendo display de izquierda y apago el de la derecha
  */
	if (digito == UNIDAD)
    {
    	digitalWrite(UNIDAD, LOW);
        digitalWrite(DECENA, HIGH);
      	delay(TIEMPO);
    }
  	else if (digito == DECENA)
    {
    	digitalWrite(UNIDAD, HIGH);
        digitalWrite(DECENA, LOW);
      	delay(TIEMPO);
    }
  	else
    {
    	digitalWrite(UNIDAD, HIGH);
        digitalWrite(DECENA, HIGH);
    }
}

//*********************************
void printCount(int count)
{
  /*
  1- Apago todos los leds
  2- mando la señal de la decena
  3- Prendo el display de la decena
  4- Apago el display
  5- Mando la señal de la unidad
  6- Prendo display de la unidad
  */
	prendeDigito(APAGADOS);	 //1
  	mostrarNumero(count/10); //2
  	prendeDigito(DECENA);    //3
  	prendeDigito(APAGADOS);  //4
  	mostrarNumero(count - 10*((int)count/10)); //5
  	prendeDigito(UNIDAD);    //6
  
}
~~~

## :robot: Link al proyecto
- [Proyecto](https://www.tinkercad.com/things/34vmM1zva4y-copy-of-stunning-lappi-kieran/editel?sharecode=F42e5XHR80Ocge0p2highHbdIEys9grwlMNyKdWlhOA).

---
### Fuentes
- [Sistemas de Procesamiento de Datos - Clase 4
](https://www.youtube.com/watch?v=_Ry7mtURGDE&t=1755s).
