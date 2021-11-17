![Directories](http://www.bytte.com.co/ftpaccess/Varios/CarlosG/Documentaci%C3%B3n/LogoBytte.png)

# Bytte SDK
# Documento Integración SDK Nativo IOS ID Dav
### CONFIDENCIALIDAD

La información contenida en el presente documento es CONFIDENCIAL, hace parte del secreto comercial e industrial de la empresa e implica transmisión de información cuya propiedad corresponde exclusivamente a BYTTE SAS. En consecuencia, la divulgación o el uso inapropiado de la información aquí contenida por parte del receptor de la misma, implicarán la aplicación de las normas legales pertinentes para su debida protección.

### 1. INTRODUCCIÓN

#### Propósito General del Documento

El presente documento tiene como finalidad dar a conocer el paso a paso de la instalación del SDK provisto por Bytte en una aplicación nativa IOS desarrollada con el IDE XCode.

#### 1.1. Objetivo

El presente documento tiene como objetivo explicar basado en un ejemplo, como realizar la integración del SDK Bytte a una aplicación Nativa IOS.

#### 1.2. Factores Limitantes

Los factores limitantes para la integración del SDK son:
* Se debe verificar la calidad de la cámara, es recomendable utilizar dispositivos con cámara que tengan la característica de “Auto Foco” habilitada.
* Se recomiendan cámaras con resolución mayores o iguales a 8 Mega Pixeles para un óptimo rendimiento.
* El SDK no funciona sobre dispositivos virtuales, únicamente sobre dispositivos físicos IPhone.

### 2. INSTALACIÓN

#### 2.1. Pre requisitos

* Para la compilación de aplicación en plataforma IOS, se requiere:
    > * XCode 12 o Superior con SDK IOS 12 o superior.

#### 2.2. Librerías Embebidas

* La aplicación debe tener las siguientes librerías como recurso embebido en el folder “Frameworks” como lo indica la siguiente imagen:

 ![Directories](http://www.bytte.com.co/ftpaccess/Varios/CarlosG/Documentaci%C3%B3n/LibreriasBytteF.png)

  > * BytteLibrarySDKComun.framework
  > * BytteLibrarySDKDocumentoV2.framework
  > * BytteLibrarySDKFaceID.framework
  > * BytteLIbrarySDKFingerID.framework
  > * Identy.framework
  > * IdentyFace.framework
  > * Microblink.framework



#### 2.3. Deshabilitar BitCode

Para el correcto funcionamiento de la aplicación, se debe deshabilitar el BitCode:

![Directories](http://www.bytte.com.co/ftpaccess/Varios/CarlosG/Documentaci%C3%B3n/Bitcode.png)

### 3. USO DE LA LIBRERIA

#### 3.1. Importar librerías

A continuación, se listan los import que la librería expone: 

  > * import BytteLIbrarySDKFingerID
  > * import BytteLibrarySDKFaceID
  > * import BytteLibrarySDKDocumentoV2
  
#### 3.2. Activación de la Licencia de Captura:

Bytte proporciona los archivos de licencia requeridos para las siguientes capturas:

  > * Captura de Huellas
  > * Captura de Selfie
  
Se deben embeber los archivos como recursos.

![Directories](http://www.bytte.com.co/ftpaccess/Varios/CarlosG/Documentaci%C3%B3n/ArchivosLicencia.png)

#### 3.3. Captura Documentos:

#### 3.3.1 Cédula de Ciudadanía (Hologramas):

##### ***Frente Cédula***
Para realizar el llamado se debe ejecutar el siguiente código:

```swift
    //Referencia
    private var vcFrontDocument: CapturaFrenteDocumento!
    //Evento click boton captura
    @IBAction func Capturar(_ sender: UIButton) {
        //Imagen de Fondo
        let imFront:UIImage! = UIImage(named:"front.png")
        
        //FrontDocument View Controller
        self.vcFrontDocument = CapturaFrenteDocumento(licenseKey: self.keyLicense, withPais: "co",
                                withImagenGuia: imFront, withPassword: "", withTimeOut: 20)
        
        self.vcFrontDocument.frontDocumentProcessDelegate=self;
        
        
        //Obtengo el Controller
        let vc:UIViewController! = self.vcFrontDocument.getFrenteDocumentViewController()
        
        //Presento View Controller
        self.present(vc, animated:false, completion:nil)
    }
```
* ***licenseKey***: cadena de texto entregada por Bytte para la activación del producto. 
* ***WithPais***: variable de país a capturar (co).
* ***WithTimeOut***: Tiempo para finalizar captura en segundos.
* ***withPassword***: Key para comprimir archivos.

El delegado a suscribir es ***CapturaFrontDocumentDelegate***

##### ***CallBack captura frente cédula de ciudadanía***

El evento de CallBack de la captura del código de barras, esta dado por el siguiente delegado:
* ***doCapturaBarCode***

```swift
    //Delegado de Captura del Frente
    func doCapturaFrontDocumentDelegate(frontResponse: FrenteDocumento!, FilePassword: String!) {
        DispatchQueue.main.async {
            if(frontResponse.statusOperacion){
                //El objeto frontResponse contiene la información de respuesta
            }
        }
        self.vcFrontDocument.frontDocumentProcessDelegate=nil;
    }
```

##### ***Reverso Cédula***
Para realizar el llamado se debe ejecutar el siguiente código:

```swift
    //Referencia
      private var vcBackDocumentAuthDoc:CapturaReversoDocumento!
    //Evento click boton captura
    @IBAction func btnCapturaReversocc(_ sender: UIButton) {
        //Imagen de Fondo
        let back:UIImage! = UIImage(named:"back_doc.png")
        //Llamado
        self.vcBackDocumentAuthDoc = CapturaReversoDocumento(licenseKey: self.keyLicense,withPais: "co", withImagenGuia:back, withPassword: "", withTimeOut: 30)
        self.vcBackDocumentAuthDoc.capturaBarCodeDelegate = self;
        //Obtengo el Controller
        let vc:UIViewController! = self.vcBackDocumentAuthDoc.getReversoDocumentViewController()
        //Presento
        self.present(vc, animated:false, completion:nil)
    }
```
* ***licenseKey***: cadena de texto entregada por Bytte para la activación del producto. 
* ***WithPais***: variable de país a capturar (co).
* ***WithTimeOut***: Tiempo para finalizar captura en segundos.

El delegado a suscribir es ***CapturaBarCodeDelegate***

##### ***CallBack captura reverso cédula de ciudadanía***

El evento de CallBack de la captura del código de barras, esta dado por el siguiente delegado:
* ***doCapturaBarCode***

```swift
    //Delegado de Captura del Reverso
    func doCapturaBarCode(_ infoDocumento: ReversoDocumento!, withPassword FilePassword: String!){
        DispatchQueue.main.async {
            //El objeto infoDocumento contiene la información de respuesta
        }
    }
```
#### 3.3.2 Tarjeta de Crédito:
Para realizar el llamado se debe ejecutar el siguiente código:

```swift
    //Referencia
      private var oCapturaCreditCard: CreditCardViewController!
    
    //Evento click boton captura
    @IBAction func btnCaptura(_ sender: Any) {
        //Imagen de Fondo
        let imFront:UIImage! = UIImage(named:"toma_doc_frontal.png")
        
        //FrontDocument View Controller
        self.oCapturaCreditCard = CreditCardViewController(licenseKey: self.keyLicense, withImagenGuia: imFront, withPassword: "", withTimeOut: 10)
        
        self.oCapturaCreditCard.capturaCreditCardDelegate=self;
        
        //Obtengo el Controller
        let vc:UIViewController! = self.oCapturaCreditCard.getCreditCardViewController()
        
        //Presento View Controller
        self.present(vc, animated:false, completion:nil)
    }
```
* ***licenseKey***: cadena de texto entregada por Bytte para la activación del producto. 
* ***WithPais***: variable de país a capturar (co).
* ***WithTimeOut***: Tiempo para finalizar captura en segundos.

El delegado a suscribir es ***CapturaCreditCardDelegate***

##### ***CallBack captura tarjeta de crédito***

El evento de CallBack de la captura de tarjeta de crédito, esta dado por el siguiente delegado:
* ***doCapturaCreditCard***

```swift
    //Delegado de Captura tarjeta de crédito
    func doCapturaCreditCard(_ oInfoCreditCard: CreditCardResponse!, withPassword FilePassword: String!) {
        DispatchQueue.main.async {
            //El objeto oInfoCreditCard contiene la información de respuesta
        }
    }
```
#### 3.4. Captura Foto:
Para realizar el llamado se debe ejecutar el siguiente código:

```swift
    //Referencia
    private var Ofoto: CapturaFotoViewController!
    
    //Evento click boton captura
    @IBAction func btnCapturar(_ sender: UIButton) {
        
        self.Ofoto=CapturaFotoViewController(x: 0, y: 0, color: 0)
        
        self.Ofoto.capturaFotoDelegate=self;
        
        //Obtengo el Controller
        let vc:UIViewController! = self.Ofoto.getFotoViewController();
        
         //Presento View Controller
        self.present(vc, animated:false, completion:nil)
    }
```
* ***x***: parámetro ancho de la imagen. 
* ***y***: parámetro alto de la imagen.
* ***Color***: parámetro monocromático 1 - imagen monocromático ok 0.

El delegado a suscribir es ***CapturaFotoDelegate***

##### ***CallBack captura foto***

El evento de CallBack de la captura foto, esta dado por el siguiente delegado:
* ***doCapturaFoto***

```swift
    //Delegado de Captura foto
    func doCapturaFoto(_ oInfoFoto: FotoResponse!) {
        DispatchQueue.main.async {
            //El objeto oInfoFoto contiene la información de respuesta
        }
    }
```
#### 3.5. Extraer Archivo:

Para realizar el llamado se debe ejecutar el siguiente código:

```swift
    //Referencia
    private var OArchivo: ExtraerArchivoViewController!
    
    //Evento click boton captura
    @IBAction func btnImagen(_ sender: UIButton) {
        
        self.OArchivo = ExtraerArchivoViewController(idTipo: 1)
        
        self.OArchivo.extraerArchivoDelegate=self;
        
        //Obtengo el Controller
        let vc:UIViewController! = self.OArchivo.getArchivoViewController();
        
        //Presento View Controller
        self.present(vc, animated:false, completion:nil)
    }
```
* ***idTipo***: identifica el tipo de busqueda 0-> file .pdf ---1->galeria .png-.jpg. 

El delegado a suscribir es ***ExtraerArchivoDelegate***

##### ***CallBack extraer archivo***

El evento de CallBack de extraer archivo, esta dado por el siguiente delegado:
* ***doExtraerArchivo***

```swift
    //Delegado de extraer archivo
     func doExtraerArchivo(_ oInfoArchivo: ArchivoResponse!) {
        DispatchQueue.main.async {
            //El objeto oInfoArchivo contiene la información de respuesta
        }
    }
```

#### 3.6. Captura Biometría:

##### ***Biometría Dactilar***
Para realizar el llamado se debe ejecutar el siguiente código:

```swift
     //Evento Capturar
    @IBAction func btnCapturar(_ sender: UIButton) {
        let fingerID = FingerID()
        //Asigno delegate
        fingerID.delegate = self
        //Request
        //Dedo base a Capturar (Mano derecha = 1,2,3,4,5) (Mano Izquierda = 6,7,8,9,10)
        let requestFinger = FingerRequest(url: Endpoints.url, key: "", finger: 2, timeOut: Endpoints.timeOut)
        //set color
        fingerID.setBoxesColor = .white
        fingerID.setSolidColor = .black
        fingerID.setBoxesTransparent = .red
        //llamado
        fingerID.IDCaptureFinger(licenseId: licenseName, viewController: self, request: requestFinger)
    }
    
```
* ***licenseId***: String archivo capturado anteriormente (3.2).
* ***viewController***: viewController actual.
* ***setBoxesColor***: setBoxesColor. 
* ***setSolidColor***: setSolidColor. 
* ***setBoxesTransparent***: setBoxesTransparent. 
 ***FingerRequest***: 
* ***url***: url proporcionada por bytte
* ***finger***: Variable dedo a capturar. 
   > * ***2***: Mano derecha.
   > * ***7***: Mano Izquierda.
* ***timeOut***: Tiempo para finalizar captura en segundos.
* ***key***: key cifrado. 

El delegado a suscribir es ***FingerIDDelegate***

##### ***CallBack captura dactilar***

El evento de CallBack de la captura dactilar, esta dado por el siguiente delegado:
* ***doCaptureFingerID***

```swift
   //Respuesta de la captura dactilar
    func doCaptureFingerID(fingerResponse: FingerResponse) {
        if(fingerResponse.StatusOperacion) {
            for fingerData in fingerResponse.FingerprintsObjects {
                
                if let p = fingerData as? FingerPrintObject
                {
                    //Si es la que me interesa
                    if p.FingerNumber == 2 {
                        self.img1.image = p.ProcessedImage
                        self.lblFinger1.text =  "HUELLA # \(String(p.FingerNumber))"
                    }
                    
                    //Si es la que me interesa
                    if p.FingerNumber == 3 {
                        self.img2.image = p.ProcessedImage
                        self.lblFinger2.text =  "HUELLA # \(String(p.FingerNumber))"
                    }
                    
                    //Si es la que me interesa
                    if p.FingerNumber == 4 {
                        self.img3.image = p.ProcessedImage
                        self.lblFinger3.text =  "HUELLA # \(String(p.FingerNumber))"
                    }
                    
                    //Si es la que me interesa
                    if p.FingerNumber == 5  {
                        self.img4.image = p.ProcessedImage
                        self.lblFinger4.text =  "HUELLA # \(String(p.FingerNumber))"
                    }
                }
                
                self.alertFinger(titulo: "CAPTURA EXITOSA", mensaje: fingerResponse.MensajeRetorno)
            }
        } else {
            self.alertFinger(titulo: "ERROR", mensaje: fingerResponse.MensajeRetorno)
        }
    }

```
#### 3.5. Captura Facial:

##### ***Biometría Facial***
Para realizar el llamado se debe ejecutar el siguiente código:

```swift
    //Captura Facial
    @IBAction func btnCapturar(_ sender: UIButton) {
        let faceID = FaceID()
        
        faceID.delegate = self
        faceID.setColor = .red
        let requestFace = FaceRequest(url: Endpoints.url, key: "", camera: 1, timeOut: Endpoints.timeOut)
        
        faceID.IDCaptureFace(licenseId: licenseName, viewcontrol: self, request: requestFace)
    }
```
* ***licenseId***: String archivo capturado anteriormente (3.2).
* ***viewcontrol***: viewController actual.
* ***color***: Color base captura. 
 ***FaceRequest***: 
* ***url***: url proporcionada por bytte
* ***timeOut***: Tiempo para finalizar captura en segundos.
* ***key***: key cifrado. 

El delegado a suscribir es ***FaceIDDelegate***

##### ***CallBack captura facial***

El evento de CallBack de la captura facial, esta dado por el siguiente delegado:
* ***doCaptureFaceID***

```swift
   //delegate facial
    func doCaptureFaceID(faceResponse: FaceResponse) {
        if(faceResponse.StatusOperacion){
            
            self.imgFace.image = faceResponse.FaceObjects.ProcessedImage
            
            self.alertFace(titulo: "CAPTURA EXITOSA", mensaje: faceResponse.MensajeOriginal,type: 0)
        } else {
            self.alertFace(titulo: "ERROR", mensaje: faceResponse.MensajeOriginal,type: 1)
        }
    }
```
### 4. Códigos de respuesta
#### 4.1. Base Respuesta
Al realizar los llamados correspondientes para todas las capturas se genera una respuesta base, donde se controlan variables del proceso de captura.

```objc
@property (nonatomic) NSString * MensajeOriginal;
@property (nonatomic) NSString * MensajeRetorno;
@property (nonatomic) NSString * CodigoOperacion;
@property (nonatomic) BOOL       StatusOperacion;
@property (nonatomic) NSString * IdProceso;
@property (nonatomic) NSString * CodigoError;
```

#### 4.2. Time Out
![Directories](http://www.bytte.com.co/ftpaccess/Varios/CarlosG/Documentaci%C3%B3n/TimeOut.png)
#### 4.3. Cancelado por el usuario
![Directories](http://www.bytte.com.co/ftpaccess/Varios/CarlosG/Documentaci%C3%B3n/Cancelado.png)
#### 4.4. Captura correcta

![Directories](http://www.bytte.com.co/ftpaccess/Varios/CarlosG/Documentaci%C3%B3n/Correcta.png)
