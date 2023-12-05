**<font  size=6>Projekt bazy danych  firmy edukacyjnej</font>**

Przedmiot: Podstawy Baz Danych
Autorzy: Maciej Nowakowski, Zuzanna Stajniak, Mateusz Lempart


**<font  size=4>Użytkownicy bazy danych:</font>**
1. Administrator
2. Dyrektor Placówki
3. Pracownik biura administracji
4. Pracownik biura dydaktyki
5. Prowadzący
6. Tłumacz
7. Student


**<font  size=4>Funkcje Użytkowników:</font>**
1. Administrator
	* Usuwanie nagrań
2. Dyrektor placówki
	* Dodawanie pracowników
	*  Indywidualne zmienianie terminów opłat
3. Pracownik biura administracji 
	* Generowanie raportów
	* Układanie planu zajęć 
	* Zmiana harmonogramu zajęć z przyczyn losowych
4. Pracownik biura dydaktyki
	* Generowanie raportów
	* Generowanie i wysyłanie dyplomu
	* Weryfikowanie czy użytkownik zaliczył dany kurs lub studia 
5. Prowadzący
	* Tworzenie sylabusa nowego przedmiotu 
	* Tworzenie webinarów
	* Zakładanie kursów
	* Sprawdzanie obecności na stacjonarnych zajęciach 
	* Weryfikacja odrabiania nieobecności na studiach. 
6. Tłumacz
7. Student 
	* Dodawanie usług do koszyka
	*  Opłacanie usług w koszyku 
	*  Zapis na zajęcia do odrabiania nieobecności na studiach

8. Każdy użytkownik
	* Możliwość przeglądania oferty kursów


**<font  size=4>Funkcje Systemowe:</font>**
* Sprawdzanie obecności zdalnej
* Sprawdzanie czy użytkownik ma dostęp do usługi
* Sprawdzenie czy zapis na daną usługę jest możliwy.


**<font  size=4>Schemat bazy danych</font>**
![[database.jpeg]]


**<font  size=4>Opis tabel</font>**

**clients**
Tabela przechowuje podstawowe dane o kliencie. Zawiera identyfikator klienta
(clientID), imię oraz nazwisko (firstName, lastName) oraz dane adresowe (adress, city, region).

Klucz główny: clientID

**orders**
Tabela przechowuje podstawowe dane o zamówieniu. Zawiera identyfikator zamówienia
(orderID), indentyfikator klienta (clientID), datę zamówienia (orderDate), link do płatności (paymentLink) oraz datę przyjęcia płatności (receiptDate).

Klucz główny: orderID
Klucz obcy: clientID (z tabelą clients)

**orderDetails**
Tabela przechowuje szczegółowe dane o zamówieniu. Zawiera identyfikator zamówienia
(orderID),identyfikator usługi w koszyku (serviceID) oraz cenę za tą usługę (price).

Klucze główne: orderID, serviceID
Klucze obce: orderID (z tabelą orders)
					serviceID (z tabelami services, meetings)

**services**
Tabela przechowuje podstawowe dane o dostępnych usługach edukacyjnych. Zawiera identyfikator usługi (serviceID), nazwę usługi (serviceName), typ usługi (type - webinar, kurs, studia), datę rozpoczęcia (startDate) oraz cenę(price).

Klucz główny: serviceID

**studies**
Tabela przechowuje podstawowe dane o studiach. Zawiera identyfikator studiów(majorID), nazwę (name), liczbę lat studiów (years) oraz liczbę zjazdów (terms).

Klucz główny: majorID
Klucz obcy: majorID (z tabelą services)

**terms**
Tabela przechowuje podstawowe dane o zjezdzie na studiach. Zawiera identyfikator studiów(majorID), identyfikator zjazdu(termID), czesne za ten zjazd (tuition), datę rozpoczęcia i zakończenia(startDate, endDate).

Klucze główne: majorID, termID
Klucz obcy: majorID (z tabelą studies)

**sylabus**
Tabela przechowuje szczegółowe dane o zjezdzie na studiach. Zawiera identyfikator zajęć (courseID), nazwę kursu (courseName), typ kursu (type), identyfikator studiów(majorID), identyfikator zjazdu(termID).

Klucz główny: courseID
Klucze obce: majorID, termID (z tabelą terms)

**grades**
Tabela przechowuje wyniki nauczania. Zawiera identyfikator klienta (clientID), identyfikator zajęć (courseID), datę rozpoczęcia i zakończenia okresu rozliczeniowego(startDate, endDate), informację o zaliczeniu zajęć (passed).

Klucz główny: clientID
Klucze obce: clientID (z tabelą clients)
					courseID (z tabelą sylabus)

**employees**
Tabela przechowuje podstawowe dane o pracowniku. Zawiera identyfikator pracownika
(employeeID), imię oraz nazwisko (firstName, lastName) oraz stanowisko (position).

Klucz główny:  employeeID

**interpreters**
Tabela przechowuje podstawowe dane o tłumaczu. Zawiera identyfikator pracownika
(employeeID) oraz język, którym się posługuje (language)

Klucz główny: employeeID
Klucz obcy:  employeeID (z tabelą meetings)

**courseDetails**
Tabela przechowuje szczegółowe dane o kursie. Zawiera identyfikator kursu (courseID), identyfikator opiekuna przedmiotu (tutorID), maksymalną liczbę uczestników (maxParticipants),
liczbę spotkań (totalMeetings), liczbę nagrań do obejrzenia (totalRecords).

Klucz główny: courseID
Klucze obce:  courseID (z tabelami sylabus, services)
					tutorID (z tabelą employees)

**meetings**
Tabela przechowuje dane o spotkaniu. Zawiera identyfikator spotkania (meetingID), identyfikator usługi (serviceID), identyfikator prowadzącego (tutorID), identyfikator tłumacza (interpreterID), datę i czas spotkania (datetime), link do spotkania online (meetingLink), cenę za pojedyncze spotkanie (singularPrice).

Klucz główny: meetingID
Klucze obce:  serviceID (z tabelami courseDetails, services)
					tutorID (z tabelą employees)
					interpreterID (z tabelą interpreters)

**attendees**
Tabela przechowuje dane o uczestnikach spotkań. Zawiera identyfikator uczestnika (attendeeID),  identyfikator spotkania (meetingID), identyfikator usługi (serviceID), informację o obecności (present) oraz indentyfikator zajęć na których odrabiano nieobecność (substituteMeetingID).

Klucze główne: attendeeID, meetingID
Klucz obcy: attendeeID (z tabelą attendees)
					meetingID (z tabelami attendees, meetings)

**recordings**
Tabela przechowuje linki do nagrań. Zawiera identyfikator spotkania (meetingID), link do nagrania (recordLInk).

Klucz główny: meetingID
Klucz obcy: meetingID (z tabelą meetings)

**access**
Tabela przechowuje dane dostępów do nagrań. Zawiera identyfikator klienta (clientID),  identyfikator spotkania (meetingID), datę końca dostępu (lastDate) oraz status obejrzenia (status).

Klucz główny: clientID
Klucz obcy: meetingID (z tabelą recordings)





