# 📘 Profile Picture Validator

## 🎯 Cel projektu

Celem projektu jest stworzenie **API**, które automatycznie waliduje zdjęcia profilowe przesyłane przez użytkowników.  
Walidacja obejmuje dwa główne etapy:

1. **Kontrola bezpieczeństwa zdjęcia (NSFW)**  
   Wykrywanie treści niedozwolonych, takich jak nagość, treści seksualne lub inne nieodpowiednie elementy.

2. **Weryfikacja, czy zdjęcie przedstawia osobę z widoczną twarzą**  
   Zapewnienie, że użytkownik przesyła prawidłowe zdjęcie profilowe.

Docelowo API zwraca prostą odpowiedź:

```json
{
  "valid": true,
  "message": "Profile picture accepted"
}
```

## Proces walidacji składa się z kilku kroków:
1. Wczytanie obrazu
Zdjęcie może pochodzić z:
uploadu użytkownika,
adresu URL,
pliku lokalnego.

Obraz jest konwertowany do formatu PIL.Image, aby był kompatybilny z modelami.

2. Sprawdzenie bezpieczeństwa (NSFW)
Do wykrywania treści nieodpowiednich używany jest model:

Falconsai/nsfw_image_detection

Model klasyfikuje obraz jako:
SAFE
UNSAFE

W projekcie logika jest uproszczona:
"NO" → obraz jest bezpieczny
"YES" → obraz narusza zasady i zostaje odrzucony

3. Weryfikacja, czy zdjęcie przedstawia osobę z twarzą
Używany model
Do sprawdzania, czy zdjęcie przedstawia osobę z widoczną twarzą, wykorzystywany jest model typu Vision-Language (VQA),
który potrafi odpowiadać na pytania dotyczące obrazu.

Model otrzymuje:
obraz,
pytanie: "Does the image contain a person with a clearly visible face?"
i generuje odpowiedź:

"yes" → twarz wykryta
"no" → twarz niewidoczna lub brak osoby
