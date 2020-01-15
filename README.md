# Transiciones o pases entre ViewControllers Swift

## Nota

Tenemos diferentes maneras de navegar entre controladores y pasar información de estos entre estos.

## Usando StoryBoard

Usando Drag and Drop podemos conectar dos controladores o mas y asignarle un id a dicha relación para ejecutarla desde el codigo con una acción o reconizer 

```swift
self.performSegue(withIdentifier: "historial", sender: p)
// Aca le paso el id de la relación creada en SB
// El segundo parametro es el sender, debemos sobrescribir el metodo prepare para hacerle saber quien envia y que


// Sobrescribir el metodo prepere
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    
    // Aca le decimos si es id historial caste el destino al controlador de destino y le pase la información
    if let id = segue.identifier, id == "historial"{
        let obj = segue.destination as! PackageEstadoViewController
        obj.objPackage = sender as? [XMLElement]
    }
	
    // Otro ejemplo
    if let id = segue.identifier, id == "searchTracking"{
        let objNav = segue.destination as! UINavigationController
        let obj = objNav.topViewController as! TrackingViewController
        obj.tracking = sender as? String
    }
}
```

## Uso por programación

```swift
// 1 - Inicializamos la clase UIStoryBoard

let objUIStoryBoard = UIStoryBoard(name: "Nombre del storyBoard por lo regular es Main", bundle: Bundle.main)

// 2 - Inicializamos el controlador donde nos dirigimos

guard let destinoControllerView = objUIStoryBoard.instantiateViewController(withIdentifier: StoryBoardID del destino) as? DestinoControllerView else{
	print("No se encontro el ID")
	return
}

// Para pasar información al destinoControllerView simplemente usamos el objeto del destino

destinoControllerView.variableQueSeEncuentraEnDestino = "Alguna Info o objeto"

// OJO si estamos en un NavigationView usamos la variable navigationController que es global en estos casos, en caso contrario arroja nil

navigationController?.pushViewController(destinoControllerView, animated: true)

// SI NO USAMOS NavigationController usamos un present

// Puedo cambiar el modo en que se muestra el modal
destinoControllerView.modalTransitionStyle = .crossDissolve

// Puedo cambiar si se muestra en pantalla completa o de otra manera
destinoControllerView.modalPresentationStyle = .fullscreen

// Run Transition
present(destinoControllerView,animated:true,completion:nil)
```

## Para remplazar toda la pantalla y 'eliminar la anterior'

```swift
let mainStoryboardIpad : UIStoryboard = UIStoryboard(name: "Main", bundle: nil)

let initialViewControlleripad : UIViewController = mainStoryboardIpad.instantiateViewController(withIdentifier: "initLogin") as UIViewController

self.window = UIWindow(frame: UIScreen.main.bounds)
self.window?.rootViewController = initialViewControlleripad
self.window?.makeKeyAndVisible()
```

## Ejemplo de la clase destino

```swift
class DestinationViewController: UIViewController {
	
	
	@IBOutlet weak var name: UILabel!
	
	var dataLlegada:String?
	
    override func viewDidLoad() {
        super.viewDidLoad()
		
		if let txt = dataLlegada{
			name.text = txt
		}

        // Do any additional setup after loading the view.
    }
	
	@IBAction func goBack(_ sender: UIButton) {
		dismiss(animated: true, completion: nil)
	}
	
}
```
