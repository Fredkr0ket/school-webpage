Php exceptions is een handige tool die je kan gebruiken voor het afhandelen van errors in je code. Ze zijn er voor bedoeld om onverwachte erros die kunnen optreden tijdens het uitvoeren van je php code op een nette manier op te vangen. hier door kun je gestructureerder je errors vast leggen en behandelen.

## Voorbeelden van exceptions
- **PDOException**
	PDOException is een exception die word gecalled waneer er iets fout gaat met een PDO class. waneer je bijvoorbeeld geen connectie met de database kan maken geeft de PDOException de juiste error aan.
- **FileNotFoundException**
	Deze uitzondering wordt gegooit waneer er een bestand niet kan worden gevonden door middel van de `fopen()` functie
- **InvalidArgumentException**
	deze error word getrowed waneer er een verkeerde soort parameter word gegeven aan een functie.

## Aangepaste exception class
```php
class CustomException extends Exception {
    public function __construct($errorMessage, $errorCode) {
        parent::__construct($errorMessage, $errorCode);
    }
}

// Voorbeeld van het gebruik van de CustomException-klasse
try {
    $errorMessage = "Dit is een aangepaste foutmelding.";
    $errorCode = 123; // Een aangepaste foutcode

    throw new CustomException($errorMessage, $errorCode);
} catch (CustomException $e) {
    echo "Aangepaste uitzondering gevangen: " . $e->getMessage() . " (Code: " . $e->getCode() . ")";
}
```

## Afhandelen van exceptions 
```php
<?php
class CSVException extends Exception {}

try {
// Controleer of het CSV-bestand bestaat
if (!file_exists('data.csv')) {
	throw new CSVException("Het bestand 'data.csv' bestaat niet.");
}

// Probeer het CSV-bestand te openen
$file = fopen('data.csv', 'r');
if ($file === false) {
	throw new CSVException("Kan het bestand 'data.csv' niet openen.");
}
  
// Lees het CSV-bestand regel voor regel
while (($row = fgetcsv($file)) !== false) {
	// Controleer of de rij onvolledig is
	if (count($row) != 3) {
		throw new CSVException("Rij is onvolledig: " . implode(', ', $row));
	}
	// Hier kun je verdere verwerking van de rij toevoegen
	$name = $row[0];
	$email = $row[1];
	$phone = $row[2];
	// Voorbeeld: afdrukken van de gelezen gegevens
	echo "Naam: $name, E-mail: $email, Telefoon: $phone" . PHP_EOL;
}

fclose($file);
} catch (CSVException $e) {
	// Vang de aangepaste uitzonderingen op
	echo "Fout: " . $e->getMessage() . PHP_EOL;
} catch (Exception $e) {
	// Vang andere uitzonderingen op
	echo "Er is een onverwachte fout opgetreden: " . $e->getMessage() . PHP_EOL;
}
	?>
```

## Best practises voor exceptions
1. **Gebruik specifieke uitzonderingen** 
	Maak aangepaste uitzonderingsklassen voor specifieke fouttypen om de leesbaarheid en begrijpelijkheid van je code te verbeteren.
2. **Gebruik try-catch blokken op de juiste plaatsen**
	Plaats try-catch blokken alleen waar je verwacht dat uitzonderingen kunnen optreden om de foutopsporing te vergemakkelijken.
3. **Documenteer uitzonderingen** 
	Voorzie documentatie voor aangepaste uitzonderingen om andere ontwikkelaars te helpen begrijpen waarom ze worden gegenereerd en hoe ze moeten worden afgehandeld. Dit bevordert samenwerking en efficiëntie.
