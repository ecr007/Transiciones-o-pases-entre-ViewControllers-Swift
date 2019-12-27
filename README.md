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
