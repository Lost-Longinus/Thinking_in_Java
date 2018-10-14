# Chapter 19 - ***Enum***  
*  switch中只能使用整数值，而enum天生具有整数值的次序，可以用于switch。  
```sh 
/**
 * Created by Pengfei Jin on 2018/10/14.
 */
public class EnumerateTest {
	Signal color = Signal.Red;
	public void change(){
		switch (color){
			case Red: color = Signal.Green;
			break;
			case Green: color = Signal.Yellow;
			break;
			case Yellow: color = Signal.Red;
			break;
		}
	}
	public String toString(){
		return "the traffic light is " + color;
	}
	public static void main(String[] args) {
		EnumerateTest trafficLight = new EnumerateTest();
		for (int i = 0; i < 8; i++) {
			System.out.println(trafficLight);
			trafficLight.change();
		}
	}
}
enum Signal{
	Red, Yellow, Green
}
//output
the traffic light is Red
the traffic light is Green
the traffic light is Yellow
the traffic light is Red
the traffic light is Green
the traffic light is Yellow
the traffic light is Red
the traffic light is Green
```
* 邮局如何处理一封邮件直至被判为死信---***职责链设计模式***  
```sh 
import java.util.Iterator;

/**
 * Created by Pengfei Jin on 2018/10/14.
 */
public class ChainOfResponsibility {
	public static void main(String[] args) {
		for(Mail mail:Mail.generator(6)){
			System.out.println(mail.details());
			PostOffice.handle(mail);
			System.out.println("***************************************");
		}
	}
}
class Mail{
	enum GeneralDelivery{YES, NO1, NO2, NO3, NO4, NO5}
	enum Scannability{UNSCANNABLE, YES1, YES2, YES3, YES4}
	enum Readability{ILLEGIBLE, YES1, YES2, YES3, YES4}
	enum Address{INCORRECT, OK1, OK2, OK3, OK4, OK5, OK6}
	enum ReturnAddress{MISSING, OK1, OK2, OK3, OK4, OK5}
	GeneralDelivery generalDelivery;
	Scannability scannability;
	Readability readability;
	Address address;
	ReturnAddress returnAddress;
	static long counter = 0;
	long id = counter++;
	@Override
	public String toString() {
		return "Mail "+id;
	}
	public String details(){
		return toString() +
				", General Delivery: " + generalDelivery +
				", Address Scannability: " + scannability +
				", Address Readability: " + readability +
				", Address Address: " + address +
				", Return Address: " + returnAddress;
	}
	public static Mail randomMail(){
		Mail m = new Mail();
		m.generalDelivery = Enums.random(GeneralDelivery.class);
		m.scannability = Enums.random(Scannability.class);
		m.readability = Enums.random(Readability.class);
		m.address = Enums.random(Address.class);
		m.returnAddress = Enums.random(ReturnAddress.class);
		return m;
	}
	public static Iterable<Mail> generator(final int count){
		return new Iterable<Mail>() {
			int n = count;
			@Override
			public Iterator<Mail> iterator() {
				return new Iterator<Mail>() {
					@Override
					public boolean hasNext() {
						return n-->0;
					}
					@Override
					public Mail next() {
						return randomMail();
					}
					public void remove(){
						throw new UnsupportedOperationException();
					}
				};
			}
		};
	}
}
class PostOffice{
	enum MailHandler{
		GENERAL_DELIVERY{
			@Override
			boolean handle(Mail mail) {
				switch (mail.generalDelivery){
					case YES:
						System.out.println("Using general delivery for " + mail );
						return true;
					default: return false;
				}
			}
		},
		MACHINE_SCAN{
			@Override
			boolean handle(Mail mail) {
				switch (mail.scannability){
					case UNSCANNABLE:
						return false;
					default:
						switch (mail.address){
							case INCORRECT: return false;
							default:
								System.out.println("Delivering "+mail+" automatically");
								return true;
						}
				}
			}
		},
		VISUAL_INSPECTION{
			@Override
			boolean handle(Mail mail) {
				switch (mail.readability){
					case ILLEGIBLE:
						return false;
					default:
						switch (mail.address){
							case INCORRECT: return false;
							default:
								System.out.println("Delivering "+mail+" normally");
								return true;
						}
				}
			}
		},
		RETURN_TO_SENDER{
			@Override
			boolean handle(Mail mail) {
				switch (mail.returnAddress){
					case MISSING:
						return false;
					default:
						System.out.println("Returning "+mail+" to sender");
						return true;
				}
			}
		};
		abstract boolean handle(Mail mail);
	}
	public static void handle(Mail mail){
		for(MailHandler handler : MailHandler.values()){
			if (handler.handle(mail)){
			    return;
			}
		}
		System.out.println(mail + " is a dead letter!!");
	}
}
//output
Mail 0, General Delivery: NO4, Address Scannability: YES4, Address Readability: YES3, Address Address: OK3, Return Address: OK5
Delivering Mail 0 automatically
***************************************
Mail 1, General Delivery: NO1, Address Scannability: UNSCANNABLE, Address Readability: ILLEGIBLE, Address Address: OK2, Return Address: OK2
Returning Mail 1 to sender
***************************************
Mail 2, General Delivery: NO4, Address Scannability: YES1, Address Readability: YES1, Address Address: OK5, Return Address: OK1
Delivering Mail 2 automatically
***************************************
Mail 3, General Delivery: NO5, Address Scannability: UNSCANNABLE, Address Readability: YES2, Address Address: OK1, Return Address: OK3
Delivering Mail 3 normally
***************************************
Mail 4, General Delivery: NO4, Address Scannability: YES4, Address Readability: YES1, Address Address: INCORRECT, Return Address: OK3
Returning Mail 4 to sender
***************************************
Mail 5, General Delivery: NO1, Address Scannability: YES3, Address Readability: YES3, Address Address: INCORRECT, Return Address: MISSING
Mail 5 is a dead letter!!
***************************************
```