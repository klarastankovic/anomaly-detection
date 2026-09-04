# Detekcija anomalija u financijskim transakcijama

Izvorni kod praktičnog dijela završnog rada "Izrada prototipa sustava za detekciju anomalija u financijskim sustavima u stvarnom vremenu primjenom metoda strojnog učenja", izrađenog na Fakultetu elektrotehnike, računarstva i informacijskih tehnologija Osijek.

Rad uspoređuje dva pristupa detekciji anomalija, algoritam izolacijskih stabala (engl. _Isolation Forest_) i duboki autoenkoder na skupu podataka Credit Card Fraud Detection (Kaggle, ULB), te na temelju usporedbe bira konačni model za jednostavan prototip koji simulira rad sustava u stvarnom vremenu.

## Metodologija u kratkim crtama
 
- Podaci se dijele kronološki na podskup za učenje, validaciju i testiranje (63:7:30), ne nasumično, kako bi se simuliralo stvarno vrijeme i izbjeglo curenje informacija iz budućnosti u prošlost.
- RobustScaler se prilagođava isključivo na skupu za učenje.
- Isolation Forest trenira se nenadzirano na cijelom skupu za učenje; autoenkoder se trenira polunadzirano, isključivo na legitimnim transakcijama.
- Oba se modela evaluiraju na validacijskom skupu prema preciznosti, odzivu, F1-mjeri, ROC-AUC i PR-AUC vrijednosti, uz poseban naglasak na PR-AUC zbog izrazite neuravnoteženosti skupa podataka.
- Na temelju usporedbe u model_comparison.ipynb, autoenkoder je odabran kao konačni model za prototip.ipynb.

Detaljno obrazloženje svake metodološke odluke nalazi se u poglavlju 4. samog rada.
 
## Pokretanje
 
Sve bilježnice pisane su za Google Colab i preuzimaju skup podataka izravno s Kagglea putem kagglehub biblioteke. Nije potrebno ručno preuzimanje.
 
Redoslijed pokretanja:
 
1. isolation-forest/isolation_forest.ipynb može se pokrenuti neovisno (Runtime → Run all). Sprema svoj istrenirani model na kraju.
2. autoencoder/autoencoder_keras_tensor.ipynb zahtijeva ručno učitavanje amount_scaler_if.pkl i time_scaler_if.pkl iz isolation_forest.ipynb. Sprema svoj istrenirani model na kraju.
3. model_comparison.ipynb zahtijeva ručno učitavanje modela istreniranih u koraku 1 i 2 (isolation_forest_model.pkl, amount_scaler_if.pkl, time_scaler_if.pkl, autoencoder_model.keras, autoencoder_threshold.pkl), budući da Colab runtime ne dijeli datoteke između zasebnih bilježnica.
4. prototip.ipynb zahtijeva ručno učitavanje amount_scaler_if.pkl, time_scaler_if.pkl, autoencoder_model.keras i autoencoder_threshold.pkl iz koraka 2.

autoencoder_pytorch.ipynb je samostalna, dodatna implementacija iste arhitekture; zahtijeva amount_scaler_if.pkl i time_scaler_if.pkl, ali ne koristi se u koracima 2–3.
 
## Korištene tehnologije
 
Python, Scikit-learn, TensorFlow/Keras, PyTorch, Pandas, NumPy, Matplotlib, Seaborn, Kagglehub.
