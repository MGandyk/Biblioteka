# 📖 Wirtualny bibliotekarz

### Spis treści

- [Opis projektu](#opisprojektu)
- [Założenia](#zalozenia)
- [Technologie](#technologie)
- [Środowisko uruchomieniowe](#srodowisko)
- [Klasy](#klasabook)


## Opis prjektu

Aplikacja do zarządzania biblioteką to system webowy, który pozwala na efektywne zarządzanie książkami, użytkownikami, wypożyczeniami oraz innymi aspektami działania biblioteki. Projekt jest skierowany równo do bibliotek szkolnych, publicznych oraz prywaynych kolekcji, umożliwiając łatwą organizację zasobów i monitorowanie ich przepływu.

## Założenia

- Ułatwienie pracy bibliotekarzom poprzez automatyzację procesów.
- Zwiększenie dostępności biblioteki dla użytkowników dzięki rozwiązaniom online.
- Poprawa organizacji i zarządzania zasobami biblioteki.

## Technologie
- Języki programowania: C#, CSS, 
- Relacyjne bazy danych: SQL
- Konteneryzacja: Docker


### Klasa Book :
Cel: 
	Reprezentuje książki w bibliotece.
Relacje:
	Jest zarządzana przez klasę LibraryContext jako część zestawu danych (DbSet<Book>).
Dlaczego?:
Tworzy rzeczywisty obiekt "Książka", przechowując jego właściwości: 
- Id: int              
- Title: string?       
- Author: string?      
- YearPublished: int   
- IsAvailable: bool

	Klasa LibartContext
Cel: 
	Reprezentuje kontekst bazy danych dla książek.

Relacje:
        Dziedziczy z klasy DbContext, co pozwala jej działać jako pośrednik między aplikacją a bazą danych.
        Zawiera kolekcję DbSet<Book>, która reprezentuje tabelę w bazie danych.
        Używana przez BooksController do wykonywania operacji CRUD na tabeli książek.

Dlaczego?:
        Abstrakcja DbContext upraszcza komunikację z bazą danych. Dzięki temu kod operujący na danych jest czystszy i bardziej zrozumiały.




### Klasa BooksController

Cel: 
	Zarządza operacjami związanymi z książkami w API.

Relacje:
        Wstrzykuje LibraryContext poprzez konstruktor, aby wykonywać operacje na danych książek.
        Udostępnia metody API do obsługi żądań dotyczących książek:
            GetBooks: Pobiera listę książek.
            AddBook: Dodaje nową książkę.
            DeleteBook: Usuwa książkę.
            UpdateAvailability: Aktualizuje status dostępności książki.

Dlaczego?:
        Koncentruje logikę aplikacji dotyczącą książek w jednym miejscu. Dzięki temu separujemy logikę kontrolera od innych części aplikacji.

### Klasa ApplicationDbContext

Cel: 
	Zarządza operacjami związanymi z użytkownikami i rolami.

Relacje:
        Dziedziczy z IdentityDbContext, co pozwala jej zarządzać danymi użytkowników i ról.
        Jest konfigurowana w pliku Program.cs do współpracy z bazą danych za pomocą Entity Framework.

Dlaczego?:
        Integracja z ASP.NET Core Identity zapewnia łatwe zarządzanie użytkownikami i rolami, co jest kluczowe dla funkcji rejestracji, 	logowania oraz kontroli dostępu.

### Klasa Program.cs

Cel: 

Klasa Program.cs odpowiada za całą konfigurację środowiska i uruchamianie aplikacj
Relacje:
        Konfiguruje LibraryContext i ApplicationDbContext jako usługi dostępne w aplikacji.
        Dodaje ASP.NET Core Identity (AddDefaultIdentity), aby umożliwić obsługę użytkowników i ról.
        Konfiguruje middleware uwierzytelniania i autoryzacji (UseAuthentication, UseAuthorization).
        Tworzy domyślne role i przypisuje role nowym użytkownikom.

Dlaczego?:
        Centralne miejsce konfiguracji zapewnia elastyczność i łatwe zarządzanie zależnościami. Dzięki temu każda część aplikacji jest poprawnie zarejestrowana i dostępna.


### ASP.NET Core Identity

Cel: 
	Zapewnia funkcjonalności uwierzytelniania i autoryzacji użytkowników.

Relacje:
        IdentityUser: Klasa bazowa reprezentująca użytkownika. Każdy użytkownik aplikacji dziedziczy jej właściwości (np. nazwa 	użytkownika, hasło).
        IdentityRole: Klasa bazowa reprezentująca role użytkowników, takie jak "Czytelnik" i "Pracownik".
        UserManager<IdentityUser> i RoleManager<IdentityRole>:
            UserManager: Zarządza użytkownikami, pozwala tworzyć i edytować użytkowników.
            RoleManager: Zarządza rolami, umożliwia przypisywanie użytkowników do ról.
Dlaczego?:

Identity jest standardowym rozwiązaniem ASP.NET Core dla systemów uwierzytelniania i autoryzacji. Ułatwia zarządzanie użytkownikami, hasłami i rolami w bezpieczny sposób.

### OpenIddict
Cel:
 	Jest to narzędzie, które pomaga w zarządzaniu logowaniem w aplikacji. Obsługuje: 
· OAuth2: Standard pozwalający aplikacjom uzyskiwać dostęp do zasobów w imieniu użytkownika. 
· OpenID Connect: Rozszerzenie OAuth2, które pomaga w uwierzytelnieniu użytkownika. 

Dlaczego:
Dzięki OpenIddict możemy np.: 
· Umożliwić użytkownikom logowanie przez nazwę użytkownika i hasło. 
· Wygenerować tokeny, które mówią, kim jest użytkownik i co może robić. 
· Ułatwić aplikacji rozpoznanie, czy użytkownik jest zalogowany i czy ma odpowiednie uprawnienia. 




1. Struktura klas i dziedziczenie
Dziedziczenie jest jednym z podstawowych elementów programowania obiektowego.
W naszym projekcie zostało użyte w kilku miejscach:
Przykłady :
public class ApplicationDbContext : IdentityDbContext
ApplicationDbContext dziedziczy z klasy IdentityDbContext, co pozwala na wykorzystanie wbudowanych funkcjonalności zarządzania użytkownikami i ról z ASP.NET Identity. Dodaje to specjalizację dla naszej aplikacji.
Wykorzystanie dziedziczenia upraszcza implementację logiki autoryzacji i rejestracji użytkowników, ponieważ możemy korzystać z gotowych metod i struktur, rozszerzając je o własne funkcjonalności.
public class LibraryContext : DbContext
Klasa LibraryContext dziedziczy po DbContext.
	LibraryContext: Klasa, którą stworzyliśmy w naszym projekcie. Jest to klasa pochodna. Przechowuje definicję struktury naszej bazy danych oraz jej konfigurację. 
DbContext: Klasa bazowa dostarczona przez Entity Framework Core.
- Odpowiada za zarządzanie połączeniem z bazą danych.
- Umożliwia wykonywanie operacji CRUD (Create, Read, Update, Delete) na obiektach w bazie danych.
- Jest fundamentem wzorca ORM (Object-Relational Mapping), który mapuje obiekty w kodzie na dane w relacyjnej bazie danych.


2. Polimorfizm

    Polimorfizm umożliwia wywoływanie metod w sposób, który zależy od konkretnej implementacji klasy pochodnej lub dostarczonej instancji obiektu.

      Przykład: ASP.NET Core używa mechanizmu wstrzykiwania zależności (Dependency Injection), który opiera się na polimorfizmie:
builder.Services.AddScoped<AuthenticationStateProvider, RevalidatingIdentityAuthenticationStateProvider<IdentityUser>>();
Tutaj klasa RevalidatingIdentityAuthenticationStateProvider jest używana jako implementacja interfejsu AuthenticationStateProvider. Dzięki temu aplikacja może korzystać z różnych implementacji, nie wiedząc o ich szczegółach.
Cel: Polimorfizm umożliwia łatwą wymienność implementacji, co jest zgodne z zasadą otwarte/zamknięte (Open/Closed Principle).

3. Hermetyzacja
Hermetyzacja polega na ukrywaniu szczegółów implementacji i eksponowaniu jedynie niezbędnych elementów przez interfejsy publiczne.
Przykład: W klasie Book:
public class Book
{
    public int Id { get; set; }
    public string? Title { get; set; }
    public string? Author { get; set; }
    public int YearPublished { get; set; }
    public bool IsAvailable { get; set; } = true; // Domyślnie książka jest dostępna
}

 Właściwości takie jak Id, Title czy Author są publiczne, co umożliwia dostęp do danych o książce, ale implementacja obsługi danych odbywa się w kontrolerach i klasach zarządzających.  W kontrolerach, takich jak BooksController, widać hermetyzację logiki zarządzania bazą danych:
private readonly LibraryContext _context;
Cel: Hermetyzacja chroni dane i logikę aplikacji przed nieautoryzowanym dostępem, co zwiększa bezpieczeństwo i czytelność kodu.

4. Interfejsy
Interfejsy pozwalają na definiowanie kontraktów, które muszą być spełnione przez klasy implementujące.
Przykład: ASP.NET Core opiera się na wykorzystaniu interfejsów w całej strukturze, np. IServiceCollection, IDbContextOptions, IAuthenticationService.
W naszym projekcie:
builder.Services.AddScoped<UserManager<IdentityUser>>();
Interfejs UserManager<IdentityUser> jest używany do zarządzania użytkownikami, a faktyczna implementacja jest dostarczana przez ASP.NET Identity.

5. Biblioteki
W  naszym projekcie wykorzystano wiele bibliotek i frameworków, które wspierają paradygmat obiektowy:
Microsoft.EntityFrameworkCore: Używany do mapowania obiektowo-relacyjnego (ORM) i zarządzania bazą danych. Biblioteka pozwala na operowanie na danych jako na obiektach.
Przykład: LibraryContext i ApplicationDbContext zarządzają bazą danych jako kolekcją obiektów (DbSet<T>).
Microsoft.AspNetCore.Identity: Biblioteka wspierająca zarządzanie użytkownikami, uwierzytelnianiem i autoryzacją.
Przykład: Role użytkowników (Czytelnik, Pracownik) są zarządzane przez IdentityRole. Biblioteki umożliwiają ponowne wykorzystanie kodu i przyspieszają rozwój aplikacji.


Podsumowanie
Nasz projekt stosuje zaawansowane techniki programowania obiektowego:
1.	Dziedziczenie – Klasa ApplicationDbContext dziedziczy z IdentityDbContext.
2.	Polimorfizm – ASP.NET Core wykorzystuje Dependency Injection do dynamicznego dobierania implementacji.
3.	Hermetyzacja – Prywatne pola _context oraz publiczne API w kontrolerach.
4.	Interfejsy – Użycie interfejsów, takich jak IServiceCollection i IDbContextOptions, do definiowania kontraktów.
5.	Biblioteki – Rozszerzenie funkcjonalności przez Microsoft.EntityFrameworkCore i Microsoft.AspNetCore.Identity.
Te wszystkie elementy świadczą o poziomie zaawansowania naszego projektu i zgodności z zasadami programowania obiektowego.
