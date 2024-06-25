# Database for hotel
Projekt z systemów baz danych na labolatoria.
Baza danych dla hotelu, przedstawiająca implementację systemu płatności, rezerwacji pokojów oraz room-service.

<!-- <style>
 p,li {
    font-size: 12pt;
  }
</style>  -->

<!-- <style>
 pre {
    font-size: 8pt;
  }
</style>  -->


---

**Autorzy:**
- Robert Kania
- Kamil Rydarowicz
---


# 1.  Zakres i krótki opis systemu

## Zakres i krótki opis systemu
Celem projektu jest stworzenie systemu obsługi hotelowej działającego w oparciu o relacyjną bazę danych. System ten ma za zadanie usprawnić zarządzanie różnymi aspektami funkcjonowania hotelu, takimi jak rezerwacje pokojów, obsługa klientów oraz monitorowanie dostępności zasobów.

## Rezerwacja pokojów
Głównym elementem systemu jest moduł rezerwacji pokojów, który umożliwia zarówno klientom jak i personelowi dokonywanie i zarządzanie rezerwacjami. Klienci będą mieli możliwość przeglądania dostępnych pokoi, sprawdzania ich dostępności w określonym terminie oraz dokonywania rezerwacji online. Personel będzie mógł zarządzać istniejącymi rezerwacjami oraz tworzyć nowe.

## Płatność
Nasz system umożliwiać będzie dokonanie płatności w postaci kilku części np. zaliczki oraz reszty kwoty rezerwacji (w skład której będą wchodzić dodatkowe usługi takie jak barek itd czy też ewentualne szkody). Dodatkowo klient będzie mógł zapłacić w wybrany przez siebie sposób - będzie mieć kilka rodzajów płatności (np karta, gotówka). Dla jego wygody każdą część wpłaty będzie mógł dokonać innym sposobem - zaliczkę kartą, a resztę np w hotelu gotówką.

## Obsługa klientów
Kolejnym ważnym elementem systemu będzie obsługa klientów. Będzie ona obejmować zarówno rejestrację przybywających gości, udzielanie informacji na temat dostępnych usług i udogodnień. Dzięki temu, hotel będzie mógł zapewnić wysoki standard obsługi, co wpłynie na zadowolenie klientów.

## Monitorowanie dostępności zasobów
Ostatnim, ale równie istotnym elementem systemu będzie możliwość monitorowania dostępności zasobów hotelowych, takich jak pokoje czy usługi dodatkowe. Dzięki temu hotel będzie mógł efektywnie zarządzać swoimi zasobami, unikając sytuacji, w której występuje niedobór lub nadmiar danego zasobu.
W rezultacie, stworzenie tego systemu pozwoli hotelowi na efektywne zarządzanie swoimi zasobami.

## Usługi gastronomiczne
Hotel będzie oferował usługi gastronomiczne takie jak room-service, zapewniając wysoką jakość posiłków i napojów, które spełnią oczekiwania nawet najbardziej wymagających gości. Dodatkowo pracownicy hotelu będą mieli możliwość usuwania oraz dodawania nowych produktów. 

# 2.	Wymagania i funkcje systemu

## Rezerwacja pokojów:
- System umożliwia przeglądanie dostępnych pokoi w określonym terminie.
- Klienci mogą dokonywać rezerwacji online, wybierając preferowane pokoje (rodzaje) i okresy pobytu.
- Personel może dodawać i anulować rezerwacje, zarządzając dostępnością pokoi.

## Obsługa klientów:
- Rejestracja przybywających gości i przypisywanie im pokoi.
- Udzielanie informacji na temat usług i udogodnień dostępnych w hotelu.

## Monitorowanie dostępności zasobów:
- System monitoruje dostępność pokoi.
- Jest możliwość dodawania oraz usuwania konkretnych pokoi
- Automatyczne aktualizowanie dostępności na podstawie dokonanych rezerwacji i zmian w stanie zasobów.

## Usługi gastronomiczne
- Usługa room service: Hotel będzie świadczył usługę room service, umożliwiając gościom zamawianie posiłków i napojów bezpośrednio do swojego pokoju.
- Dodawanie nowych i usuwanie produktów

## System płatności
- System umożliwia kilka rodzajów płatności
- Klienci będą mogli opłacić należną kwotę w kilku częściach (choćby zaliczka i reszta kwoty)
- Każda płatność może być wykonana innym sposobem płatności

# 3.	Projekt bazy danych

## Schemat bazy danych

<img src="https://github.com/PoteznySquad/db-project/blob/main/schema.png">

## Opis poszczególnych tabel

customers:
- Opis: tabela zawierające dane klientów.

| Nazwa atrybutu | Typ          | Opis/Uwagi            |
|----------------|--------------|-----------------------|
| customerid     | int          | primary key           |
| firstname      | varchar(255) | imię klienta          |
| lastname       | varchar(255) | nazwisko klienta      |
| address        | varchar(255) | adres klienta         |
| phone          | varchar(255) | telefon klienta       |
| city           | varchar(255) | miasto klienta        |
| country        | varchar(255) | kraj klienta          |
| post_code      | varchar(255) | kod pocztowy klienta  |
| region         | varchar(255) | region klienta        |
| birthdate      | date         | data urodzin klienta  |
| pesel          | varchar(11)  | pesel klienta         |
| photopath      | varchar(255) | zdjęcie klienta       |
| notes          | text         | notatka o kliencie    |
| fax            | varchar(255) | fax klienta           |

orders:
- Opis: tabela zawierająca zamówienia

| Nazwa atrybutu   | Typ            | Opis/Uwagi             |
|------------------|----------------|------------------------|
| orderid          | int            | primary key            |
| orderdate        | date           | data zamówienia        |
| status           | bit            | status zamówienia      |
| tip              | decimal(10,2)  | napiwek                |
| discount         | decimal(10,2)  | zniżka                 |
| reservationid    | int            | fk dla reservations    |

payment_methods:
- Opis: tabela zawierająca metody płatności

| Nazwa atrybutu   | Typ          | Opis/Uwagi             |
|------------------|--------------|------------------------|
| payment_methodid | int          | primary key            |
| name             | varchar(255) | nazwa płatności        |

payments:
- Opis: tabela zawierająca płatność

| Nazwa atrybutu   | Typ           | Opis/Uwagi             |
|------------------|---------------|------------------------|
| paymentid        | int           | primary key            |
| advance          | bit           | zaliczka               |
| reservationid    | int           | fk dla orders          |
| payment_methodid | int           | fk dla payment_methods |
| payment_date     | date          | data wpłaty            |
| value            | decimal(10,2) | wartość płatności      |

products:
- Opis: tabela zawierająca produkty

| Nazwa atrybutu   | Typ           | Opis/Uwagi                  |
|------------------|---------------|-----------------------------|
| productid        | int           | primary key                 |
| unitprice        | decimal(10,2) | cena produktu               |
| unitsinstock     | int           | ilość produktów             |
| unitsinorder     | int           | ilość zamówionych produktów |
| prouctname       | varchar(255)  | nazwa produktu              |

reservated_rooms
- Opis: tabela łącznikowa między rezerwacjami i pokojami

| Nazwa atrybutu     | Typ           | Opis/Uwagi             |
|--------------------|---------------|------------------------|
| reservated_roomid  | int           | primary key            |
| roomid             | int           | fk dla roomid          |
| reservationid      | int           | fk dla reservationid   |
| price              | decimal(10,2) | cena rezerwacji        |

reservations:
- Opis: tabela zawierająca rezerwacje.

| Nazwa atrybutu | Typ            | Opis/Uwagi        |
|----------------|----------------|-------------------|
| reservationid  | int            | primary key       |
| customerid     | int            | fk dla customers  |
| start_date     | date           | data zameldowania |
| end_date       | date           | data wymeldowania |
| note           | varchar(255)   | uwagi             |
| additional     | decimal(10,2)  | cena za uwagi     |

room_type
- Opis: tabela zawierająca typy pokojów

| Nazwa atrybutu   | Typ           | Opis/Uwagi             |
|------------------|---------------|------------------------|
| room_typid       | int           | primary key            |
| beds             | int           | ilość łóżek w pokoju   |
| persons          | int           | ilość osób na pokój    |
| description      | varchar(255)  | opis pokoju            |
| price            | decimal(10,2) | cena pokoju            |

rooms:
- Opis: tabela zawierająca pokoje

| Nazwa atrybutu   | Typ          | Opis/Uwagi             |
|------------------|--------------|------------------------|
| roomid           | int          | primary key            |
| room_typeid      | int          | fk dla roomtypeid      |
| number           | varchar(255) | numer pokoju           |

processed_orders:
- Opis: tabela łącznikowa między zamówieniami i produktami

| Nazwa atrybutu    | Typ           | Opis/Uwagi             |
|-------------------|---------------|------------------------|
| processed_orderid | int           | primary key            |
| orderis           | int           | fk dla orderid         |
| productid         | int           | fk dla productid       |
| price             | decimal(10,2) | cena za produkt        |

# 4.	Implementacja

## Kod poleceń DDL

```sql
CREATE TABLE customers (
    customerid int  NOT NULL IDENTITY,
    firstname varchar(255)  NOT NULL,
    lastname varchar(255)  NOT NULL,
    address varchar(255)  NOT NULL,
    phone varchar(255)  NOT NULL,
    city varchar(255)  NOT NULL,
    country varchar(255)  NOT NULL,
    post_code varchar(255)  NULL,
    region varchar(255)  NULL,
    birthdate date  NOT NULL,
    pesel varchar(11)  NOT NULL,
    photopath varchar(255)  NULL,
    notes text  NULL,
    fax varchar(255)  NULL,
    CONSTRAINT customers_pk PRIMARY KEY  (customerid)
);
```

```sql
CREATE TABLE orders (
    orderid int  NOT NULL,
    orderdate date  NOT NULL IDENTITY,
    status bit  NOT NULL,
    tip decimal(10,2)  NOT NULL,
    discount decimal(10,2) NULL,
    reservationid int  NOT NULL,
    CONSTRAINT orders_pk PRIMARY KEY  (orderid)
);
```

```sql
CREATE TABLE payment_methods (
    payment_methodid int  NOT NULL IDENTITY,
    name varchar(255)  NOT NULL,
    CONSTRAINT payment_methodid PRIMARY KEY  (payment_methodid)
);
```

```sql
CREATE TABLE payments (
    paymentid int  NOT NULL IDENTITY,
    advance bit  NOT NULL,
    reservationid int  NOT NULL,
    payment_methodid int  NOT NULL,
    payment_date date  NOT NULL,
    value decimal(10,2)  NOT NULL,
    CONSTRAINT payments_pk PRIMARY KEY  (paymentid)
);
```

```sql
CREATE TABLE processed_orders (
    processed_orderid int  NOT NULL IDENTITY,
    orderid int  NOT NULL,
    productid int  NOT NULL,
    price decimal(10,2) NOT NULL,
    CONSTRAINT processed_orders_pk PRIMARY KEY  (processed_orderid)
);
```

```sql
CREATE TABLE products (
    productid int  NOT NULL IDENTITY,
    unitprice decimal(10,2)  NOT NULL,
    unitsinstock int  NOT NULL,
    unitsinorder int  NOT NULL,
    productname varchar(255)  NOT NULL,
    CONSTRAINT products_pk PRIMARY KEY  (productid)
);
```

```sql
CREATE TABLE reservated_rooms (
    reservated_roomid int  NOT NULL IDENTITY,
    roomid int  NOT NULL,
    reservationid int  NOT NULL,
    price decimal(10,2)  NOT NULL,
    CONSTRAINT reservated_rooms_pk PRIMARY KEY  (reservated_roomid)
);
```

```sql
CREATE TABLE reservations (
    reservationid int  NOT NULL IDENTITY,
    customerid int  NOT NULL,
    start_date date  NOT NULL,
    end_date date  NOT NULL,
    note varchar(255)  NULL,
    additional decimal(10,2)  NOT NULL,
    CONSTRAINT reservations_pk PRIMARY KEY  (reservationid)
);
```

```sql
CREATE TABLE room_type (
    room_typeid int  NOT NULL IDENTITY,
    beds int  NOT NULL,
    persons int  NOT NULL,
    description varchar(255)  NULL,
    price decimal(10,2)  NOT NULL,
    CONSTRAINT room_type_pk PRIMARY KEY  (room_typeid)
);
```

```sql
CREATE TABLE rooms (
    roomid int  NOT NULL IDENTITY,
    room_typeid int  NOT NULL,
    number varchar(255)  NOT NULL,
    CONSTRAINT rooms_pk PRIMARY KEY  (roomid)
);
```

```sql
ALTER TABLE orders ADD CONSTRAINT orders_reservations
    FOREIGN KEY (reservationid)
    REFERENCES reservations (reservationid);
```

```sql
ALTER TABLE payments ADD CONSTRAINT payments_payment_methods
    FOREIGN KEY (payment_methodid)
    REFERENCES payment_methods (payment_methodid);
```

```sql
ALTER TABLE payments ADD CONSTRAINT payments_reservations
    FOREIGN KEY (reservationid)
    REFERENCES reservations (reservationid);
```

```sql
ALTER TABLE processed_orders ADD CONSTRAINT processed_orders_orders
    FOREIGN KEY (orderid)
    REFERENCES orders (orderid);
```

```sql
ALTER TABLE processed_orders ADD CONSTRAINT processed_orders_products
    FOREIGN KEY (productid)
    REFERENCES products (productid);
```

```sql
ALTER TABLE reservated_rooms ADD CONSTRAINT reservated_rooms_reservations
    FOREIGN KEY (reservationid)
    REFERENCES reservations (reservationid);
```

```sql
ALTER TABLE reservated_rooms ADD CONSTRAINT reservated_rooms_rooms
    FOREIGN KEY (roomid)
    REFERENCES rooms (roomid);
```

```sql
ALTER TABLE reservations ADD CONSTRAINT reservations_customers
    FOREIGN KEY (customerid)
    REFERENCES customers (customerid);
```

```sql
ALTER TABLE rooms ADD CONSTRAINT rooms_room_type
    FOREIGN KEY (room_typeid)
    REFERENCES room_type (room_typeid);
```

## Widoki

1. Widok wypisuje liste klientów i zarezerwowane przez nich pokoje

```sql
CREATE VIEW customer_reservations AS
SELECT 
    c.customerid,
    c.firstname,
    c.lastname,
    c.phone,
    c.city,
    c.country,
    r.reservationid,
    r.start_date,
    r.end_date,
    r.additional
FROM 
    customers c
JOIN 
    reservations r ON c.customerid = r.customerid;
```

2. Widok wypisuje szczegółowe dane odnośnie zamówionych posiłków i napoji

```sql
CREATE VIEW order_details AS
SELECT 
    o.orderid,
    o.orderdate,
    o.status,
    o.tip,
    o.discount,
    po.productid,
    p.productname,
    p.unitprice
FROM 
    orders o
JOIN 
    processed_orders po ON o.orderid = po.orderid
JOIN 
    products p ON po.productid = p.productid;
```

3. Widok wypisuje informacje o dokonanych płatnościach przez klientów

```sql
CREATE VIEW payment_summary AS
SELECT 
    p.paymentid,
    r.customerid,
    c.firstname,
    c.lastname,
    pm.name AS payment_method,
    p.payment_date,
    p.value,
    p.advance
FROM 
    payments p
JOIN 
    reservations r ON p.reservationid = r.reservationid
JOIN 
    customers c ON r.customerid = c.customerid
JOIN 
    payment_methods pm ON p.payment_methodid = pm.payment_methodid;
```

4. Widok wypisuje dane o klientach którzy muszą zapłacić dodatkowo za usterki

```sql
CREATE VIEW customers_with_additional_charges AS
SELECT 
    c.customerid,
    c.firstname,
    c.lastname,
    c.phone,
    c.city,
    c.country,
    r.reservationid,
    r.start_date,
    r.end_date,
    r.additional,
    r.note
FROM 
    customers c
JOIN 
    reservations r ON c.customerid = r.customerid
WHERE 
    r.additional > 0;
```

5. Widok wypisuje cenę i numery pokoi których nie zarezerwowano

```sql
CREATE VIEW available_rooms AS
SELECT r.roomid, r.number, rt.price
FROM rooms r
LEFT JOIN reservated_rooms rr ON r.roomid = rr.roomid
JOIN room_type rt ON r.room_typeid = rt.room_typeid
WHERE rr.roomid IS NULL;
```

6. Widok wypisuje klientów którzy zamówili więcej niż jeden produkt

```sql
CREATE OR ALTER VIEW Customers_With_Multiple_Orders AS
SELECT
    c.customerid,
    c.firstname,
    c.lastname
FROM
    customers c
    JOIN reservations r ON c.customerid = r.customerid
    JOIN orders o ON r.reservationid = o.orderid
    JOIN processed_orders po ON o.orderid = po.processed_orderid
GROUP BY
    c.customerid,
    c.firstname,
    c.lastname
HAVING
    COUNT(DISTINCT o.orderid) > 1;
```

## Procedury

1. Procedura dodawania nowego pokoju do tabeli rooms.

```sql
CREATE procedure add_room
    @room_typeid int,
    @number varchar(255)
as
begin
    if exists (select 1 from room_type where room_typeid = @room_typeid)
        begin
            insert into rooms (room_typeid, number)
            values (@room_typeid, @number);
        end
    else
        begin
            RAISERROR('nie ma takiego typu pokoju: %d', 16, 1, @room_typeid);
        end
end;
go
```
2. Procedura dodawania nowego klienta do tabeli customers.

```sql
create procedure add_customer
@firstname varchar(255),
@lastname varchar(255),
@address varchar(255),
@phone varchar(255),
@city varchar(255),
@country varchar(255),
@post_code varchar(255),
@region varchar(255),
@birthdate date,
@pesel varchar(11),
@photopath varchar(255),
@notes text,
@fax varchar(255)
as
begin
    if LEN(@pesel) <> 11
        begin
            RAISERROR('PESEL musi miec 11 cyfr.', 16, 1)
            return
        end

    insert customers(firstname,lastname,address,phone,city,country,post_code,region,birthdate,pesel,photopath,notes,fax)
    values(@firstname,@lastname,@address,@phone,@city,@country,@post_code,@region,@birthdate,@pesel,@photopath,@notes,@fax)
end
go
```
3. Procedura dodająca nowy produkt

```sql
create procedure add_product
    @unitprice int,
    @unitsinstock int,
    @unitsinorder int,
    @productname varchar(255)
as
begin
    if exists (select 1 from products where productname = @productname)
        begin
            RAISERROR ('Nie mozna dodac produktu - produkt o takiej nazwie juz istnieje.', 16, 1);
            return
        end
    else
        begin
            insert into products (unitprice, unitsinstock, unitsinorder, productname) values (@unitprice, @unitsinstock, @unitsinorder, @productname);
        end
end
go
```
4. Procedura usuwająca produkt.
   
```sql
create procedure remove_product
    @productid int
as
begin
    if exists (select 1 from products where productid = @productid)
        begin
            delete from products where productid = @productid;
        end
    else
        begin
            RAISERROR('Nie istnieje produkt o takim id.', 16, 1)
            return;
        end
end
go
```
5. Procedura usuwająca pokój.
   
```sql
CREATE procedure remove_room
@roomid int
as
begin
    if exists (select 1 from reservated_rooms where roomid = @roomid)
        begin
            RAISERROR ('Nie można usunąć zarezerwowaengo pokoju.', 16, 1)
            return;
        end
    else
        begin
            delete from rooms where roomid = @roomid;
        end
end
go
```
6. Procedura dodająca płatność
   
```sql
CREATE procedure add_payment
    @advance bit,
    @reservationid int,
    @payment_methodid int,
    @payment_date date,
    @value decimal
as
begin
    if exists (select 1 from reservations where reservationid = @reservationid)
    and exists (select 1 from payment_methods where payment_methodid = @payment_methodid)
        begin
            insert into payments (advance, reservationid, payment_methodid, payment_date, value)
            values (@advance, @reservationid, @payment_methodid, @payment_date, @value)
        end
    else
        begin
            RAISERROR ('Nie istnieje rezerwacja o podanym id, lub podano niepoprawna metode platnosci.', 16, 1);
            return
        end
end
go
```
7. Procedura dodająca rezerwację (dodająca również pokój do zarezerwowanych)
```sql
CREATE procedure add_reservation
    @customerid int,
    @start_date date,
    @enddate date,
    @note varchar(255),
    @additional decimal,
    @roomid int,
    @price decimal
as
begin
    insert into reservations(customerid, start_date, end_date, note, additional)
    values (@customerid, @start_date, @enddate, @note, @additional)

    declare @reservationID int
    set @reservationID = scope_identity()

    exec add_reserved_room @reservationID, @roomid, @price
end
go
```
8. Procedura dodająca do konkretnej rezerwacji kolejny pokój
```sql
CREATE PROCEDURE add_reserved_room
    @reservationid INT,
    @roomid INT,
    @price DECIMAL
AS
BEGIN
    if exists (select 1 from reservated_rooms where roomid = @roomid)
        begin
            RAISERROR ('Pokoj o takim ID jest juz zarezerwowany.', 16, 1)
            return
        end
    else
        begin 
            INSERT INTO reservated_rooms (reservationid, roomid, price)
    VALUES (@reservationid, @roomid, @price);
        end
END;
go
```

## Funkcje

1. Funkcja obliczająca całość kwoty do zapłaty przez klienta za rezerwację (w tym dodatki, szkody)

```sql
CREATE FUNCTION total_price()
    RETURNS TABLE
        AS
        RETURN (
        WITH RoomCosts AS (
            SELECT
                r.customerid,
                SUM(rr.price) AS room_total
            FROM reservations r
                     JOIN reservated_rooms rr ON r.reservationid = rr.reservationid
            GROUP BY r.customerid
        ),
             OrderCosts AS (
                 SELECT
                     r.customerid,
                     SUM(po.price * (1 - ISNULL(o.discount, 0))) AS order_total,
                     SUM(o.tip) AS tip_total
                 FROM reservations r
                          JOIN orders o ON r.reservationid = o.reservationid
                          JOIN processed_orders po ON o.orderid = po.orderid
                 GROUP BY r.customerid
             ),
             AdditionalCosts AS (
                 SELECT
                     r.customerid,
                     SUM(r.additional) AS additional_total
                 FROM reservations r
                 GROUP BY r.customerid
             ),
             PaymentTotals AS (
                 SELECT
                     r.customerid,
                     SUM(p.value) AS payment_total
                 FROM reservations r
                          JOIN payments p ON r.reservationid = p.reservationid
                 GROUP BY r.customerid
             )
        SELECT
            c.customerid,
            c.firstname,
            c.lastname,
            ISNULL(rc.room_total, 0) +
            ISNULL(oc.order_total, 0) +
            ISNULL(oc.tip_total, 0) +
            ISNULL(ac.additional_total, 0) -
            ISNULL(pt.payment_total, 0) AS total_price
        FROM customers c
                 LEFT JOIN RoomCosts rc ON c.customerid = rc.customerid
                 LEFT JOIN OrderCosts oc ON c.customerid = oc.customerid
                 LEFT JOIN AdditionalCosts ac ON c.customerid = ac.customerid
                 LEFT JOIN PaymentTotals pt ON c.customerid = pt.customerid
        )
go
```
## Kod wstawiający przykładowe dane

- Customers
```sql
INSERT INTO rokaniaa.dbo.customers (customerid, firstname, lastname, address, phone, city, country, post_code, region, birthdate, pesel, photopath, notes, fax) VALUES (1, N'Robert', N'Kania', N'Krolestwo 69', N'123456789', N'Wadowice', N'Polska', N'120-34', N'Doniczka Wielka', N'1997-06-14', N'22336677892', null, null, null);
INSERT INTO rokaniaa.dbo.customers (customerid, firstname, lastname, address, phone, city, country, post_code, region, birthdate, pesel, photopath, notes, fax) VALUES (2, N'Natan', N'Marcoń', N'ul. Garażowa 17', N'344748378', N'Warszawa', N'Polska', N'00-001', N'Mazowieckie', N'1998-05-03', N'98483748593', N'./p_images/natanmarcon1.png', null, N'43453483123');
INSERT INTO rokaniaa.dbo.customers (customerid, firstname, lastname, address, phone, city, country, post_code, region, birthdate, pesel, photopath, notes, fax) VALUES (3, N'Denis', N'Załęcki', N'ul. Urzędnicza 34', N'438573847', N'Toruń', N'Polska', N'87-106', N'Kujawsko-Pomorskie', N'1995-02-27', N'34913475371', N'./p_images/zaleckidenis1.png', null, N'34819374138');
INSERT INTO rokaniaa.dbo.customers (customerid, firstname, lastname, address, phone, city, country, post_code, region, birthdate, pesel, photopath, notes, fax) VALUES (4, N'Kamil', N'Rydziu', N'ul. Beskidzka 13', N'341938341', N'Cweliska', N'Polska', N'29-134', N'Wielkopolska', N'1992-01-30', N'34813841834', N'./p_images/qjeqwi.png', null, N'31828312884');
INSERT INTO rokaniaa.dbo.customers (customerid, firstname, lastname, address, phone, city, country, post_code, region, birthdate, pesel, photopath, notes, fax) VALUES (5, N'Michał', N'Kowalkiewicz', N'ul. Obozowa 13', N'431883148', N'Kraków', N'Polska', N'30-341', N'Małopolskie', N'2003-11-13', N'34190834134', N'./p_images/jdfksjfe.png', null, null);
INSERT INTO rokaniaa.dbo.customers (customerid, firstname, lastname, address, phone, city, country, post_code, region, birthdate, pesel, photopath, notes, fax) VALUES (6, N'Władysław', N'Załęcki', N'ul. Skośna 1', N'314314319', N'Kraków', N'Polska', N'30-343', N'Małopolskie', N'2024-06-14', N'31348318414', null, null, null);
INSERT INTO rokaniaa.dbo.customers (customerid, firstname, lastname, address, phone, city, country, post_code, region, birthdate, pesel, photopath, notes, fax) VALUES (7, N'Grzegorz', N'Szczurek', N'ul. Krótka 43', N'341831845', N'Głogoczów', N'Polska', N'30-293', N'Małopolskie', N'1992-12-23', N'93148184314', null, N'Klient uczulony na bataty', null);
INSERT INTO rokaniaa.dbo.customers (customerid, firstname, lastname, address, phone, city, country, post_code, region, birthdate, pesel, photopath, notes, fax) VALUES (8, N'Marcin', N'Majkut', N'ul. Idolowo 25', N'341831484', N'Warszawa', N'Polska', N'00-005', N'Mazowieckie', N'1997-03-16', N'31431431844', null, null, null);
INSERT INTO rokaniaa.dbo.customers (customerid, firstname, lastname, address, phone, city, country, post_code, region, birthdate, pesel, photopath, notes, fax) VALUES (9, N'Darin', N'Nikalow', N'ul. Krym 85', N'413831944', N'Krym', N'Ukraina', N'443-03', N'Krym', N'1979-09-16', N'98314893434', null, null, null);
INSERT INTO rokaniaa.dbo.customers (customerid, firstname, lastname, address, phone, city, country, post_code, region, birthdate, pesel, photopath, notes, fax) VALUES (10, N'Bartek', N'Mikro', N'al. Powstancow 45', N'783567321', N'Krakow', N'Polska', N'540-45', N'Maloposlka', N'2004-06-12', N'32167857633', null, null, null);
INSERT INTO rokaniaa.dbo.customers (customerid, firstname, lastname, address, phone, city, country, post_code, region, birthdate, pesel, photopath, notes, fax) VALUES (11, N'Robert', N'Gorat', N'al. Beczkowa 21', N'987467231', N'Katowice', N'Polska', N'321-45', N'Wielkopolska', N'2002-06-04', N'12897567321', null, N'Klient uczulony na orzechy.', null);
INSERT INTO rokaniaa.dbo.customers (customerid, firstname, lastname, address, phone, city, country, post_code, region, birthdate, pesel, photopath, notes, fax) VALUES (12, N'Kamil', N'Figura', N'al. Sluchwki 90', N'674567333', N'Waraszawa', N'Polska', N'900-20', N'Dolnoslaskie', N'1998-06-04', N'89945687633', null, null, null);
INSERT INTO rokaniaa.dbo.customers (customerid, firstname, lastname, address, phone, city, country, post_code, region, birthdate, pesel, photopath, notes, fax) VALUES (13, N'Weronika', N'Sowa', N'al. Frizowo 69', N'456784932', N'Gdansk', N'Polska', N'320-45', N'Dolnoslaskie', N'2000-06-04', N'34456788890', null, null, null);
INSERT INTO rokaniaa.dbo.customers (customerid, firstname, lastname, address, phone, city, country, post_code, region, birthdate, pesel, photopath, notes, fax) VALUES (14, N'Denis', N'Kulka', N'al. Buchaja 60', N'567896444', N'Katowice', N'Polska', N'890-32', N'Jujawsko-pomorskie', N'1992-06-06', N'23456721190', null, N'Klient ma cukrzyce.', null);
INSERT INTO rokaniaa.dbo.customers (customerid, firstname, lastname, address, phone, city, country, post_code, region, birthdate, pesel, photopath, notes, fax) VALUES (15, N'Mikolaj', N'Fijor', N'al. Dulczoka 34', N'456799112', N'Bielsko-Biala', N'Polska', N'240-87', N'Mazowieckie', N'2004-06-12', N'34567884321', null, null, null);
INSERT INTO rokaniaa.dbo.customers (customerid, firstname, lastname, address, phone, city, country, post_code, region, birthdate, pesel, photopath, notes, fax) VALUES (16, N'Natan', N'Boja', N'al. Pokito 48', N'467765432', N'Wadowice', N'Polska', N'456-34', N'Slaskie', N'1995-06-10', N'34678765436', null, null, null);
INSERT INTO rokaniaa.dbo.customers (customerid, firstname, lastname, address, phone, city, country, post_code, region, birthdate, pesel, photopath, notes, fax) VALUES (17, N'Grzegorz', N'Szwaja', N'ul. Kumkata 69', N'456543567', N'Glogoczow', N'Polska', N'321-45', N'Slaskie', N'2005-06-16', N'87652346735', null, N'Klient ma downa.', null);
INSERT INTO rokaniaa.dbo.customers (customerid, firstname, lastname, address, phone, city, country, post_code, region, birthdate, pesel, photopath, notes, fax) VALUES (18, N'Michal', N'Romaszewski', N'ul. Posledna 80', N'543223589', N'Zadupie', N'Polska', N'69-69', N'Pomorskie', N'2011-06-10', N'53213698965', null, N'Klient mocno uposledzony.', null);
```

- Orders
```sql
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-09-18', 1, 29.00, 0.10, 1);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-08-02', 1, 12.00, 0.20, 2);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-07-10', 0, 87.00, null, 3);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-07-12', 1, 50.00, null, 3);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-11-23', 0, 71.00, null, 5);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-05-30', 1, 0.00, 0.30, 6);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-05-29', 0, 5.00, null, 6);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-02-23', 0, 83.00, 0.35, 9);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-03-28', 1, 52.00, 0.20, 9);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-08-11', 1, 0.00, null, 10);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-05-01', 0, 53.00, null, 12);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-03-03', 0, 27.00, 0.15, 13);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-08-10', 0, 32.00, 0.10, 13);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-01-14', 1, 0.00, 0.25, 14);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-09-09', 1, 31.00, null, 15);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-11-30', 0, 55.00, 0.10, 17);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-06-06', 1, 0.00, 0.10, 18);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-07-01', 1, 23.00, null, 20);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-09-20', 1, 52.00, null, 20);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-12-02', 0, 44.00, null, 20);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-05-20', 0, 20.00, 0.15, 21);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-08-17', 1, 91.00, 0.20, 22);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-10-25', 1, 0.00, null, 24);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-09-06', 0, 93.00, 0.25, 25);
INSERT INTO rokaniaa.dbo.orders (orderdate, status, tip, discount, reservationid) VALUES (N'2023-10-02', 0, 12.00, null, 25);
```

- Payment methods
```sql
INSERT INTO rokaniaa.dbo.payment_methods (name) VALUES (N'Gotówka');
INSERT INTO rokaniaa.dbo.payment_methods (name) VALUES (N'Blik');
INSERT INTO rokaniaa.dbo.payment_methods (name) VALUES (N'Karta');
INSERT INTO rokaniaa.dbo.payment_methods (name) VALUES (N'Przelew');
```

- Payments
```sql
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (1, 1, 2, N'2023-09-01', 1279.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (1, 2, 1, N'2023-07-02', 380.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (1, 3, 4, N'2023-06-05', 452.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (1, 4, 2, N'2024-06-15', 669.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (1, 5, 2, N'2023-11-11', 840.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (1, 6, 1, N'2023-05-02', 247.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (1, 7, 3, N'2023-12-08', 590.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (1, 8, 4, N'2023-09-10', 588.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (1, 9, 2, N'2023-01-01', 344.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (1, 10, 4, N'2023-06-03', 837.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 11, 4, N'2023-02-12', 1184.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 12, 1, N'2023-04-20', 1585.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 13, 3, N'2023-02-03', 1650.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 14, 2, N'2023-01-10', 1487.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 15, 3, N'2023-07-12', 1817.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 16, 1, N'2023-08-23', 1656.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 17, 4, N'2023-11-16', 1818.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 18, 3, N'2023-05-11', 1306.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 19, 2, N'2023-04-01', 1084.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 20, 4, N'2023-06-20', 1321.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 21, 4, N'2023-05-09', 1581.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 22, 3, N'2023-07-03', 1720.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 23, 1, N'2023-05-23', 1332.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 24, 2, N'2023-10-03', 1981.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 25, 4, N'2023-08-30', 1460.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 1, 2, N'2023-09-03', 866.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 2, 3, N'2023-07-15', 1191.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 3, 1, N'2023-06-21', 614.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 4, 3, N'2024-06-22', 820.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 5, 4, N'2023-11-19', 1094.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 6, 4, N'2023-05-08', 1381.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 7, 1, N'2023-12-10', 346.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 8, 2, N'2023-09-12', 623.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 9, 2, N'2023-01-05', 1218.00);
INSERT INTO rokaniaa.dbo.payments (advance, reservationid, payment_methodid, payment_date, value) VALUES (0, 10, 2, N'2023-06-12', 396.00);
```

- Processed Orders
```sql
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (1, 11, 5);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (1, 1, 34);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (2, 9, 10);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (4, 8, 69);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (6, 11, 8);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (6, 2, 7);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (6, 4, 5);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (19, 14, 10);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (9, 11, 4);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (10, 1, 27);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (10, 2, 7);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (10, 13, 9);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (1, 14, 9);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (14, 3, 32);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (15, 4, 8);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (15, 9, 10);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (17, 9, 11);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (18, 4, 10);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (14, 11, 8);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (18, 11, 7);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (22, 13, 7);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (22, 4, 3);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (23, 2, 9);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (23, 10, 8);
INSERT INTO rokaniaa.dbo.processed_orders (orderid, productid, price) VALUES (14, 14, 10);
```

- Products
```sql
INSERT INTO rokaniaa.dbo.products (unitprice, unitsinstock, unitsinorder, productname) VALUES (30.00, 93, 2, N'Wino');
INSERT INTO rokaniaa.dbo.products (unitprice, unitsinstock, unitsinorder, productname) VALUES (8.00, 173, 3, N'Piwo');
INSERT INTO rokaniaa.dbo.products (unitprice, unitsinstock, unitsinorder, productname) VALUES (35.00, 34, 1, N'Pizza');
INSERT INTO rokaniaa.dbo.products (unitprice, unitsinstock, unitsinorder, productname) VALUES (5.00, 321, 4, N'Wode');
INSERT INTO rokaniaa.dbo.products (unitprice, unitsinstock, unitsinorder, productname) VALUES (100.00, 50, 0, N'Whisky');
INSERT INTO rokaniaa.dbo.products (unitprice, unitsinstock, unitsinorder, productname) VALUES (24.00, 64, 0, N'Burger');
INSERT INTO rokaniaa.dbo.products (unitprice, unitsinstock, unitsinorder, productname) VALUES (20.00, 43, 0, N'Spaghetti');
INSERT INTO rokaniaa.dbo.products (unitprice, unitsinstock, unitsinorder, productname) VALUES (70.00, 78, 1, N'Wódka');
INSERT INTO rokaniaa.dbo.products (unitprice, unitsinstock, unitsinorder, productname) VALUES (10.00, 122, 3, N'Frytki');
INSERT INTO rokaniaa.dbo.products (unitprice, unitsinstock, unitsinorder, productname) VALUES (7.00, 54, 1, N'Lemoniada');
INSERT INTO rokaniaa.dbo.products (unitprice, unitsinstock, unitsinorder, productname) VALUES (6.00, 244, 5, N'Czekolada');
INSERT INTO rokaniaa.dbo.products (unitprice, unitsinstock, unitsinorder, productname) VALUES (25.00, 69, 0, N'Ciasto');
INSERT INTO rokaniaa.dbo.products (unitprice, unitsinstock, unitsinorder, productname) VALUES (8.00, 155, 2, N'Pepsi');
INSERT INTO rokaniaa.dbo.products (unitprice, unitsinstock, unitsinorder, productname) VALUES (8.00, 132, 3, N'Coca cola');
```

- Reservated Rooms
```sql
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (4, 2, 1500.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (7, 4, 1500.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (8, 7, 1700.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (14, 8, 550.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (20, 11, 1650.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (30, 14, 1150.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (26, 17, 1200.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (15, 22, 1000.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (1, 24, 700.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (5, 13, 1650.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (9, 1, 500.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (11, 6, 1400.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (21, 9, 730.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (24, 15, 1300.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (29, 25, 1150.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (17, 21, 1200.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (3, 19, 1050.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (27, 3, 560.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (2, 5, 1150.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (28, 10, 1490.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (13, 12, 1800.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (19, 16, 1500.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (10, 18, 600.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (6, 20, 1450.00);
INSERT INTO rokaniaa.dbo.reservated_rooms (roomid, reservationid, price) VALUES (16, 23, 500.00);
```

- Reservations
```sql
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (5, N'2023-09-05', N'2023-10-20', N'Stłuczony wazon', 262.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (4, N'2023-07-19', N'2023-08-10', null, 0.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (2, N'2023-06-23', N'2023-08-14', null, 0.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (8, N'2024-06-23', N'2023-06-28', N'Porysowane meble', 206.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (3, N'2023-11-21', N'2023-12-04', N'Wybita okna w szybie', 297.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (6, N'2023-05-09', N'2023-06-08', null, 0.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (7, N'2023-12-11', N'2023-12-30', null, 0.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (14, N'2023-09-14', N'2023-12-07', null, 0.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (9, N'2023-01-09', N'2023-04-16', null, 0.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (10, N'2023-06-16', N'2023-12-15', N'Pęknięty ekran telewizora', 790.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (11, N'2023-02-14', N'2023-11-16', null, 0.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (13, N'2023-04-26', N'2023-06-17', null, 0.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (7, N'2023-02-18', N'2023-09-20', null, 0.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (8, N'2023-01-13', N'2023-02-20', N'Rozbitych 5 szklanek', 318.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (6, N'2023-07-22', N'2023-11-04', null, 0.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (16, N'2023-08-30', N'2023-11-07', null, 0.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (17, N'2023-11-27', N'2023-12-12', N'Poplamiony dywan', 307.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (15, N'2023-05-12', N'2023-07-06', N'Niedziałający prysznic', 238.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (2, N'2023-04-21', N'2023-05-10', N'Pęknięte lustro', 439.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (1, N'2023-06-27', N'2023-12-16', null, 0.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (14, N'2023-05-14', N'2023-06-26', N'Zgubiony klucz', 343.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (13, N'2023-07-06', N'2023-09-17', null, 0.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (18, N'2023-05-28', N'2023-06-22', null, 0.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (17, N'2023-10-23', N'2023-11-24', N'Podarte poduszki', 410.00);
INSERT INTO rokaniaa.dbo.reservations (customerid, start_date, end_date, note, additional) VALUES (12, N'2023-08-31', N'2023-10-27', null, 0.00);
```

- Room type
```sql
INSERT INTO rokaniaa.dbo.room_type (beds, persons, description, price) VALUES (1, 1, N'Pokój jednoosobowy z jednym łóżkiem', 200.00);
INSERT INTO rokaniaa.dbo.room_type (beds, persons, description, price) VALUES (1, 2, N'Pokój dwuosobowy z jednym łóżkiem', 250.00);
INSERT INTO rokaniaa.dbo.room_type (beds, persons, description, price) VALUES (2, 2, N'Pokój dwuosobowy z dwoma łóżkami', 350.00);
INSERT INTO rokaniaa.dbo.room_type (beds, persons, description, price) VALUES (2, 3, N'Pokój dwuosobowy z trzema łóżkami(jedno podwójne)', 400.00);
INSERT INTO rokaniaa.dbo.room_type (beds, persons, description, price) VALUES (3, 3, N'Pokój trzyosobowy z trzema łóżkami', 450.00);
```

- Rooms
```sql
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (1, N'1A');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (2, N'2A');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (2, N'3A');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (3, N'4A');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (4, N'5A');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (4, N'1B');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (5, N'2B');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (5, N'3B');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (1, N'4B');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (1, N'5B');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (3, N'1C');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (4, N'2C');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (5, N'3C');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (1, N'4C');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (2, N'5C');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (1, N'6A');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (2, N'7A');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (4, N'8A');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (4, N'9A');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (5, N'10A');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (1, N'6B');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (5, N'7B');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (3, N'8B');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (3, N'9B');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (4, N'10B');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (2, N'6C');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (1, N'7C');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (3, N'8C');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (2, N'9C');
INSERT INTO rokaniaa.dbo.rooms (room_typeid, number) VALUES (2, N'10C');
```





