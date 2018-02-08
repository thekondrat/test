Попытался добавить этот коментарий в блог http://privateblog.info/prostoj-sposob-upravleniem-servoprivodom-sg90/#comment-1086
Но там возникла какая то ошибка. Поэтому я зпихнул этот комент сюда.
__


Пробовал использовать приведенный пример в своей задаче:
ардуина принимает ИК сигнал и в зависимости от этого меняет направление вращения сервы.
У меня ничего не вышло (
Полагаю это связанно с тем что используется напрямую обращение к ногам процессора?(так это называется?)
Так же мне пришлось изменить int main(void)  на void setup() т.к. в противном случае у меня ничего не компилилось.

Если вызывать функцию angle передавая ей в качестве параметра число то все работает. Но как только я пробую передать туда переменную всё ломается (



Для упарвления ИК приемником использую эту библиотеку https://github.com/z3t0/Arduino-IRremote


#define MIN_PULSE_WIDTH       1125    
#define MAX_PULSE_WIDTH      1925
#define ANGLE_PULSE_WIDTH      100
#define PULSE_WIDTH         20000  

#define PORT      0x08 //Digital 3 (D3)

#include "IRremote.h"
IRrecv irrecv(2); // указываем вывод, к которому подключен приемник
decode_results results;


void setup() {
  int angle_test=3;
 // pinMode(13, OUTPUT);
  irrecv.enableIRIn(); // запускаем прием
  DDRD = PORT; //Set D3 as output
  while(1){
     if ( irrecv.decode( &results )) { // если данные пришли
          switch ( results.value ) {
              case 0xFFA857:
                //digitalWrite( 13, HIGH );
                angle_test=1;
                break;
              case 0xFFE01F:
                //digitalWrite( 13, LOW );
                angle_test=7;
                break;
          }    
          irrecv.resume(); // принимаем следующую команду
    }
    angle(angle_test);   // если вызывать функцию с числом а не с переменной то всё ок
 //   angle(3);   // параметр функции задает скорость вращения, у меня серва 360
}
}

void angle(int anglef) {
  
    PORTD = 0x00;
    PORTD = PORT;
    //_delay_us(MIN_PULSE_WIDTH);
    _delay_us(MIN_PULSE_WIDTH + ANGLE_PULSE_WIDTH * anglef);
    PORTD = 0x00;
    _delay_us(PULSE_WIDTH - MAX_PULSE_WIDTH);
}


--
Anton Belov
