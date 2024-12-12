# Prediction-for-day-trading
make an AI, which decide to buy or short

#What is it used for
Ez a kód megjósolja hogy egy adott coin értéke nőni, vagy csökkenni fog egy óra elteltével az aktuális árhoz képest, így segíti a felhasználót, hogy buy vagy short tranzakciót hajtson végre. 

#the dataset used for the model
a fileok között, 'doge_dataset(15min).xlsx'. Ez az adatsor kellően hosszú, hogy alapjáúl szolgálljon a modell tanításának, így minél tőbb alakzatot lefedve.

#modell
A programhoz LSTM alapú keras-t használtunk, mivel idősoros adatokkal dolgozunk. 
A tanítóadatot egy excel fileból töltjük be a modellba, amik egy adott (jelen esetben a doge) coin 15 percenkénti árát tartalmazza. Ezeket 0 és 1 közé skálázzuk, majd szegmensekre osztjuk, ami a lookback érték. Így 4 szegmens pontosan 1 órát fog lefedni. Az így kapott adattsorból, azt feltételezve hogy az árfolyamváltozásokban rövidtávon szabályos alakzatok lelhetők fel, a modell megtanulja ezeket és ezek alapján prediktál. A predikció után a modell 4*15 perces, azaz egy órás szegmensekben fogja a kimenetet is adni, amit aztán visszaskáláz az eredeti tartományba.

#Prediction_app
Ez az app a már betanított modellt használja 1 óra múlvai predikció készítésére. Ki tudjuk választani a prediktálni kívánt adatsort, mely kellő mennyiségű adatot és ezeket 15 perces felbontásban tartalmazza. Fontos még, hogy a program a záróárakat nézi, ami azt jelenti, hogy csak a 15 perces záróadatokat tartalmazó adatot tartalmazó file alapján készíti az elvárt predikciót. (Természetesen a 15 percestől eltérő felosztást is lehet használni, ekkor pl 5 perc esetén 5*4 perc azaz 20 perc múlvára fogja jósolni az értéket, habár a kimeneten továbbra is 1 óra szerepel, ezt a felhasználónak kell figyelembe vennie.) A modellt is ki kell választani amivel jósolni szeretnénk, ezt magunk is legenerálhatjuk, de egy már meglévő modell is található a fileok között ('buy_or_short.keras').

#installtion
Ha a meglévő modellt szeretnénk használni akkor először a parancssorban töltsük le a következő kellékeket az alábbi kóddal:

pip install tkinter pandas numpy tensorflow matplotlib scikit-learn

Fontos a lehető legújabb python verzió, és a "Add Python 3.x to PATH" opció, különben nem fog lefutni a program.  Ezután töltsük le az appot és a modellt.

#Use
ha letöltöttünk minden szükséges filet, az app megnyitásávan egy ablak jelenik meg, ahol kiválaszthatjuk a modellt (ajánlott: 'buy_or_short.keras'), ezután a prediktálni kívánt adatsort .xlsx kiterjesztésben. Az ablakkal együtt megnyílik a parancssor is, ahol a végső predikción ttovábbi információk listázódnak ki a program futása közben, amiket így könnyen nyomon követhetünk.

#Test
Az app predikcióinak teszteléséről egy excel munkalap található a fileok között

