# 432mhz-daljinsko-upravljanje-rkpa-ddohm

Daljinski upravljač 4 tipke - u praksi je 432 MHz odašiljač koda

Ovaj RF odašiljač je veoma malen što se tiče dimenzija te ima širok spektar radnog napona (3V-12V).
Ovaj RF odašiljač ima domet do 100m, imajte na umu da je ovo samo u savršenim uvjetima te da na ovo 
utječe mnogo stvari od kojih su među presudnima prepreke između transmitera i Receivera, 
napon baterije transmiter te dizajn antene. Koristan je za korištenje na kraćim udaljenostima. 
Ovaj daljinski radi na frekvenciji od 433 MHz te ih je veoma jednostavno spojiti na nekakav mikrokontroler. 
RF receiver je također veoma malih dimenzija, ovaj jeftini receiver se koristi za primanje RF signala na 
specificiranoj frekvenciji. Super regenerativni dizajn osigurava osjetljivost na slabi signal. 
Ima 4 pina, no zapravo nam treba samo 3 pina, koristit ćemo GND VCC te jedan data pin. 
Kako bismo ostvarili bežični prijenos podataka transmitter i receiver moraju raditi u paru kako bi 
komunicirali jedan s drugim.

• Frekvencija: 433MHz
• Modulacija: ASK
• Izlaz na prijemniku: HIGH – 1/2 od VCC, LOW – 0.7V
• Napon na odašiljaču: 3V – 12V (veći napon = veći doseg)
• Domet: Max. 100m u zatvorenom prostoru, 100m na otvorenom prostoru (za bolji doseg koristiti antenu(zalemiti na mjesto ANT na pločicama) te veći napon na odašiljaču)

U ovom predavanju radit ćemo s 433Mhz Transcievereom (daljinski s 4 gumba) te Receiveru koji ćemo spojiti s Arduinom.

Kako bismo povezali Dasduino s recieverom koristit ćemo 3 pina s recievera od kojih su 2 pina za napajanje te jedan pin 
za prijenos podataka (desni data pin na slici ispod). 
Te ćemo koristiti napajanje s Dasduina te jedan digital pin
(D2 pin na kojem se ujedno nalazi jedan od 2 interrupta na Dasduinu).

Gnd –> GND DASDUINO

VCC –> +5V DASDUINO

DATA1—>D2 (interrupt)

Kako bismo mogli koristiti receiver moramo naučiti Dasduino kako komunicirati s modulom tj. 
moramo dodati rc-switch library iz kojeg ćemo koristiti funkcije za komunikaciju s modulom.
1. Downloadajte library s linka : https://github.com/sui77/rc-switch

   3. Kako bismo doznali koje kodove šalje daljinski moramo Uploadati example koji će nam preko
   4.  serial monitora prikazati sve kodove koji se odašilju na 433MHz u
   5. neposrednoj blizini te na istoj modulaciji signala.
  
   6. Primijetite kako se pojavljuju 4 različita koda te kako ima 4 različite tipke što znači da smo uspješno prikupili sve kodove koji će nam u budućnosti trebati.

 

DASDUINO KOD
Kako bismo mogli nastaviti na ovaj korak morate doznati sve iz koraka Dodavanja librarija ako to niste učinili vratite se natrag te nastavite nakon što ste učinili sve iz prethodnog koraka.
Nakon što ste doznali koji kod je povezan s kojom tipkom to možemo veoma lako iskoristiti. U ovome primjeru ćemo iskoristiti 2 tipke na daljinskom (tipka gore te tipka dolje) kako bismo uključivali i isključivali led diodu, ovu led diodu veoma lako možemo zamijeniti s relejom solid state relejom koji će pokretati puno veće trošilo.

#include <RCSwitch.h>  //dodavanja skinutog librarija
RCSwitch mySwitch = RCSwitch();
void setup() {//funckija koja se pokreće samo jednom
  mySwitch.enableReceive(0);  // Receiver na interrupt pinuz 0 => digital pin 2
  pinMode(LED_BUILTIN,OUTPUT);//deklaracija ugrađene ledice kao izlaza
}
void loop() {//funkcija koja se pokreće sve dok je mikrokontroler uključen
  if (mySwitch.available()) {//ako se podatci primaju
    
    int value = mySwitch.getReceivedValue();//spremi podatke u varijablu
    if( mySwitch.getReceivedValue()==5592332)//ako je decimalna vrijednost primljenih podataka jednaka
  {
    digitalWrite(LED_BUILTIN,HIGH);//uključi ugrađenu ledicu
    }
    else if( mySwitch.getReceivedValue()==5592512))//ako je decimalna vrijednost primljenih podataka jednaka
      {
    digitalWrite(LED_BUILTIN,LOW);//isključi ugrađenu ledicu
    }
  
   
    mySwitch.resetAvailable();//Nakon što su se podatci primili resetiraj funkciju
  }
}
Ako ste sve prethodne korako učinili uspješno onda biste trebali vidjeti iduće prizore

Nakon pritiska na gumb “gore” ugrađena LED dioda se isključi

Nakon pritiska na gumb “dolje” ugrađena LED dioda se uključi



3. 
