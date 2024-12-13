# Prediction-for-day-trading
#make an AI, which decide to buy or short

#What is it used for

Ez a kód megjósolja hogy egy adott coin értéke nőni, vagy csökkenni fog egy óra elteltével az aktuális árhoz képest, így segíti a felhasználót, hogy buy vagy short tranzakciót hajtson végre. 

#the dataset used for the model

A fileok között, 'doge_dataset(15min).xlsx'. Ez az adatsor kellően hosszú, hogy alapjáúl szolgálljon a modell tanításának, így minél tőbb alakzatot lefedve.

#modell

A programhoz LSTM alapú keras-t használtunk, mivel idősoros adatokkal dolgozunk. 
A tanítóadatot egy excel fileból töltjük be a modellba, amik egy adott (jelen esetben a doge) coin 15 percenkénti árát tartalmazza. Ezeket 0 és 1 közé skálázzuk, majd szegmensekre osztjuk, ami a lookback érték. Így 4 szegmens pontosan 1 órát fog lefedni. Az így kapott adattsorból, azt feltételezve hogy az árfolyamváltozásokban rövidtávon szabályos alakzatok lelhetők fel, a modell megtanulja ezeket és ezek alapján prediktál. A predikció után a modell 4*15 perces, azaz egy órás szegmensekben fogja a kimenetet is adni, amit aztán visszaskáláz az eredeti tartományba.
https://colab.research.google.com/drive/1mJ5I8nMvDQH7GtPVPkHSH1qRTqVf2DGK?usp=sharing
Ezen a munkalapon megtalálható a modell, illetve egy azt használó (és a Prediction app alapjául szolgáló) egyszerűbb program is.

#Prediction_app

Ez az app a már betanított modellt használja 1 óra múlvai predikció készítésére. Ki tudjuk választani a prediktálni kívánt adatsornak egy excel filet, mely kellő mennyiségű adatot és ezeket 15 perces felbontásban tartalmazza. Fontos még, hogy a program a záróárakat nézi, ami azt jelenti, hogy csak a 15 perces záróadatokat tartalmazó adatot tartalmazó file alapján készíti az elvárt predikciót, azaz pontosan az excel file close oszlopa alapján. (Természetesen a 15 percestől eltérő felosztású adatsort is lehet használni, ekkor pl 5 perc esetén 5*4 perc azaz 20 perc múlvára fogja jósolni az értéket, habár a kimeneten továbbra is 1 óra szerepel, ezt a felhasználónak kell figyelembe vennie.) A modellt is ki kell választani amivel jósolni szeretnénk, a már meglévő modell a fileok között található ('buy_or_short.keras'). Az app kimenete egyetlen érték, mely alapján eldönti hogy venni, vagy eladni érdemes az általunk választott coint. Ez az érték az 1 óra múlvai záróár, amit fel is tüntet a predikcióban.


#installtion

Ajánlott python 3.10.9-es verziót használmi, és a "Add Python 3.x to PATH" opciónak is be kell legyen pipálva telepítésnél, különben nem fog lefutni a program.
Ha a meglévő modellt szeretnénk használni akkor először a parancssorban töltsük le a következő kellékeket az alábbi kóddal:

 pip install pandas==2.2.3 
 pip install numpy==1.26.0 
 pip install tensorflow==2.18.0 
 pip install scikit-learn==1.5.2 
 pip install matplotlib==3.9.3 

Ezután töltsük le az appot és a modellt, valamint a tesztadatot.

#Use

Ha letöltöttünk minden szükséges filet, az app megnyitásávan egy ablak jelenik meg, ahol kiválaszthatjuk a modellt (ajánlott: 'buy_or_short.keras'), ezután a prediktálni kívánt adatsort .xlsx kiterjesztésben. Az ablakkal együtt megnyílik a parancssor is, ahol a végső predikción túl további információk listázódnak ki a program futása közben, amiket így könnyen nyomon követhetünk.

#Test

Az app predikcióinak teszteléséről egy excel munkalap található a fileok között, ami 2024.12.11  1:45:00-től 2024.12.12  1:15:00-ig tartalmazza a Tron coin adott időre vonatkozó értékeit, ahol az első oszlop az időpont, az 5 oszlop pedig az általunk figyelt előző időszakasz záróára. A teszt úgy készült, hogy a programnak csak az első 16 sort, azaz 5:15:00-ig mutattuk meg az értékeket, ez alapján készített egy predikciót a következő 1 óra múlvai értékre. Ez fel is van tüntetve a 'teszt eredmények' oszlopban, valamint össze van vetve, hogy a prediktált ár hány százaléka az eredeti árnak. Ezután a program az első 20 sort valódi adatai alapján készített predikciót és igy atovább a nap végéig. Minden prediktált ár fel van tüntetve az erre kijelölt oszlopban, és egy külön cellában, hogy a jósolt adatok összesen hány százalékban fedik le a valósat. Ez alapján a modell igen közeli értékeket prediktált, bár ez lehet annak az oka is, hogy a tron a figyelt időszakban pont hasonló mozgásokat mutatott, mint a tanítóadatsorban használt coin. Lehet hogy más körülmények, illetve piaci viszonyok mellett nem lennének ilyen közeliek a predikciok. A program pontosságát további tesztekkel lehetne még pontosítani.

