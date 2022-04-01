# TrafficSignDetection
Progetto di Machine (Deep) Learning con l'obiettivo di realizzare Traffic Sign Detection a partire dal GTSDB 

## Info sul progetto
Questo è il notebook che contiene il codice con l'obiettivo di addestrare una rete neurale convoluzionale (CNN) per effettuare Object Detection nel caso specifico di cartelli stradali. Il dataset usato è il [GTSDB](https://benchmark.ini.rub.de/). L'architettura scelta è quella di YOLOv3 sulla quale viene effettuato transfer learning (tramite un freeze di darknet 53) in fase di addestramento. La rete, dopo aver identificato il cartello, lo classifica in base a 4 differenti classi: obbligo, divieto, pericolo e altro.

### Descrizione dettagliata della rete
YOLOv3 è una versione migliorata di YOLO e YOLOv2 a cura di Joseph Redmon e Ali Farhadi della University of Washington. YOLOv3 è una FCN (Fully Convolutional Network) di 106 layer pre-addestrata sul COCO dataset e implementata tramite le librerie per deep learning di Keras e OpenCV. Rispetto alle precedenti architetture (RCNNs) la rete si presenta come un blocco monolitico dove l’estrazione delle feature, box regression (localizzazione degli oggetti) e classificazione vengono unificate. A differenza quindi dei modelli precedenti con due layer di output, uno per la distribuzione di probabilità della classe e l’altro per le predizioni dei box, in YOLO il tutto è riunito in un singolo output. 

La rete prende ispirazione da ResNet e dall’architettura FPN (Feature Pyramid Network), implementando come feature extractor la sottorete pre addestrata su ImageNet Darknet-53 (52 layer convoluzionali), contiene skip connection e tre output di predizione, ciascuno dei quali processa l’immagine a scale differenti, tramite dei downsampling rispettivamente di 32, 16 e 8 delle dimensioni delle immagini di input. La detection fatta su scale differenti risolve in parte alcune criticità nel riconoscimento di oggetti di piccole dimensioni e ci è sembrata ideale come applicazione al nostro problema riguardante nel particolare i cartelli stradali.<br>

Paper di riferimento sulla rete [YOLOv3: An Incremental Improvement](https://pjreddie.com/media/files/papers/YOLOv3.pdf).

### Installazione librerie e prerequisiti
Per eseguire il codice di seguito è necessario installare le seguenti librerie: 
tensorflow, numpy, pandas, matplotlib, seaborn e opencv-python (usando anche il comando pip install). Non è escluso che altre possano essere necessarie.

È necessario che questo file sia in una directory che contiente le directory TrainIJCNN2013 e TestIJCNN2013 (che contengono i file .ppm con i dati di training e test). Sono scaricabili dal sito [GTSDB](https://benchmark.ini.rub.de/). Inoltre è necessario che diverse directory esistano e devono essere quindi create manualmente quando scritto.

È inoltre necessario che alcune directory (anche vuote) esistano nella directory corrente. Queste sono tutte quelle che si vedranno nelle prime righe di ogni cella, in cui vengono configurati i path.  

La parte di creazione del modello è indipendente dalla precedente e usa solamente i file tfrecord creati dalla prima parte oltre ai file delle immagini convertite in JPG. È possibile quindi testare l'addestramento con diversi Hyperparametri senza dover ri-creare i file tfrecord e ripetere le fasi iniziali.

Le informazioni sul codice se brevi sono scritte come commenti all'inizio di ogni cella oppure se più consistenti come cella Markdown precedente al codice.  

Per semplificare l'utilizzo del codice è stato usato il modulo di logging di python. Questo permette di stampare in una finestra di console i messaggi di log. Per modificare il livello di "verbosità" usare logging.setVerbosity("info") o logging.setVerbosity("warning") dato che sono gli unici utilizzati. Gli errori critici stampano sul stdout usando le normali print. Questo per mantenere un output più pulito e facile da leggere. In caso di errori meglio passare al livello info per avere più dettagli per debuggare.
