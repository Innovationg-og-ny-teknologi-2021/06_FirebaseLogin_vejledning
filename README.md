# 5_firebase-database_vejledning

## Slut produkt
https://user-images.githubusercontent.com/48329669/128408720-8449ae85-8722-4f91-8dc7-0fe58191fb75.mp4

## Integration med Firebase
1. Start med at oprette et nyt projekt.
2. Installér følgende dependencies med `npx expo install eller npm install`;
   1. `npx expo install @react-navigation/native @react-navigation/bottom-tabs @react-navigation/stack react-native-vector-icons firebase`
3.  Opret nu en authentication-database i Firebase;
- Følg dette link: https://firebase.google.com/
- Tryk på "Get Started"
- Tryk på "Add Project"
- Giv projektet et vilkårligt navn og tryk "continue"
- Fjern Analytics og tryk "create project"
- Registrer nu en  web Aapplikation. Tryk på ikonet </> - Se billeder herunder;
  ![img_1](https://user-images.githubusercontent.com/55731954/128145049-ee6b0029-1372-45d6-b6ec-0fefaf49e18b.png)

- Giv applikationen et vilkårligt navn og tryk "Register app".

- Gå ind under real time database, og tryk create database. Tryk nu videre og vælg testmode. Gå nu til settings og gå ned i bunden og kopier firebaseConfig-kodestykket nedenfor
<img width="708" alt="Skærmbillede 2022-09-26 kl  11 31 44" src="https://user-images.githubusercontent.com/111279752/192243274-8a1c87ae-3417-4221-8a48-20327db70874.png">

- Indsæt dette i app.js, som ved sidste øvelse

## Opret app struktur
1. Opret nu følgende 3 komponenter og brug skabelon 1 i alle tre
   - Add_edit_Car.js 
   - CarDetails.js
   - CarList.js
2. Husk at importere de nødvendige komponenter fra node modules, som React og Text fra react native, præcis som i plejer i         alle filer
3. Husk også at komponentnavnet skal være ens med filnavnet


## metro.config.js og Firebase v.9
Da firebase blev opdateret her i slut August 2022, kan der stadig være bugs som skaber problemer. Et af dem er følgende
`ERROR MESSAGE: "While trying to resolve module 'idb'..... Indeed none of these files exist":`

Denne fejl vil i muligvis få, når i forsøger at compile jeres applikation på jeres mobil.  

Hvis i modtager den fejl så læs om løsning hertil her: https://stackoverflow.com/questions/72179070/react-native-bundling-failure-error-message-while-trying-to-resolve-module-i

## App.JS - Opret React navigation
1. Importer de nødvendige komponenter som vi plejer
   2. Hint: ```import { getApps, initializeApp } from "firebase/app"; ```
2. Efter din const af firebase Configarationen, skal Firebase initialiseres:
   1. ``` 
      if (getApps().length < 1) {
       initializeApp(firebaseConfig);
       console.log("Firebase On!");
       // Initialize other firebase products here
        }
    ```
3. Initialiser Stack navigatoren med `const Stack = createStackNavigator();`
4. Lav derefter en funktion kaldet StackNavigation, som skal returnere 3 Screens med "name" samt komponentnavnene CarList, CarDetails og Add_edit_car
   1. Det er vigtigt at CarList placeres øverst ;)
   2. Se evt. https://reactnavigation.org/docs/stack-navigator/
   3. Du kan også se eksempler i tidligere øvelser
5. Oppe ved `const Stack = createStackNavigator()`, Initialiser deraf en Bottom navigatorer med `const Tab = createBottomTabNavigator();`
6. Gå nu til return, og ligesom i de sidste navigation øvelser, så opret en ``<NavigationContainer></NavigationContainer>``, som skal wrappe din Tab.navigator --> Se https://reactnavigation.org/docs/bottom-tab-navigator/ 
7. I Tab.Navigator wrapper oprettes to Tab.Screen med name og component, som henholdvis skal være StackNavigation og Add_edit_Car
8. Tilføj et ikon til Home Tab.Screen | Eksempel ( husk at Ionicons skal importeres)
``<Tab.Screen name={'Home'} component={StackNavigation} options={{tabBarIcon: () => ( <Ionicons name="home" size={20} />),headerShown:null}}/>``
9. Tilføj nu også et ikon til "Add" Screenen, hvor ikon navnet er ``add`` istedet for home
10. Nu burde du kunne trykke imellem add og stacknavigatoren "Carlist"
11. Tips: Hvis du får fejl, så læs hvad react native brokker sig over, og kopier det ind i Google og brug stackOverFlow

## Add_edit_car komponent 
1. Import de nødvendige komponenter som vi plejer
   2. Hint: ```import {useEffect, useState} from "react"; import { getDatabase, ref, child, push, update  } from "firebase/database"; ```
2. I parameter parantesen indsæt nu `{navigation,route}` i stedet for `props`, så vi  henter fra react navigations parametre som skal sendes videre fra forskellige Screens
   3. Hint: `const Add_edit_Car = ({navigation,route}) => {}`
3. Lav øverst en db constant med `const db = getDatabase();`
4. Lav derefter en initialState object med tilhørende string attributter: brand,model,year og licensplate
   1. ``const initialState = {
      brand: '',
      model: '',
      year: '',
      licensePlate: ''
      }``
5. **States** |Lav nedenunder en ny State kaldet `[newCar,setNewCar]`, hvor du i useState bruger `initialState`
6. Lav derunder en const funktion kaldet isEditCar: `const isEditCar = route.name === "Edit car"` eller hvad du nu har kaldt dette screen name i stacknavigatoren i app.js
   1. ( bruges til senere )
7. Opret nu en changeTextInput funktion
   ``` 
   const changeTextInput = (name,event) => {
        setNewCar({...newCar, [name]: event});
   }
   ```
8. **Content |** Gå til return og lav et SafeAreaView som parent, og deri et ScrollView
   1. Nu laver vi et über powermove med JS, hvor vi i vores return funktion skal returnere en række felter som vi skal kunne indtaste og ændre værdier i
      1. Start med i ScrollView'et at åbne eksekveringen af JS med {}, og deri lav et Object keys funktion af newCar, som har en funktion med parametrene for attributterne model osv og index. 
      2. Se eventuelt nede i hints  `powermove JS funktion 2`
      3. Opret et return og opret først et `View` med styling, deri et `Text` komponent som deri tager attributterne navnet 
      4. Derefter opret et `TextInput` med value, onChangeText og style. Value skal være lig newCar[parameternavnet] og onChangeText skal tage event og kalde en changeSelect-funktion, som tager newCar parameter navn og event med som parameter - Igen: se hint 2
   2. lav nu en button som har en save changes funktion kaldet handlesave(), og giv en title `Add car`
9. **HandleSave** | Lav en handleSave funktion, som først henter newCar objektets keys brand, model, year `const {brand,model .... etc } = newCar;`
   1. Opret et if, som kigger på om brand og de andre attributter har en længde på 0 for at tjekke for tomme felter fx `brand.length === 0` , og return en `Alert.alert()` med en besked
   2. Kopier koden fra hints 3 og placer den nedenunder. Prøv at forstå hvad der sker i funktionen 
10. Nå du trykker på button i din app burde du nu få en alert med, at du har saved. Gå nu ind i firebase (console.firebase.com) for at se, om din bil er blevet oprettet
11. Hint: 
```
return (
        <SafeAreaView style={styles.container}>
            <ScrollView>
                {
                    Object.keys(initialState).map((key,index) =>{
                        return(
                            <View style={styles.row} key={index}>
                                <Text style={styles.label}>{key}</Text>
                                <TextInput
                                    value={????[key]}
                                    onChangeText={(event) => changeTextInput(key,event)}
                                    style={styles.input}
                                />
                            </View>
                        )
                    })
                }
                {/*Hvis vi er inde på edit car, vis save changes i stedet for add car*/}
                <Button title={ isEditCar ? "Save changes" : "Add car"} onPress={() => handleSave()} />
            </ScrollView>
        </SafeAreaView>
    );
```


### CarList
1. Importer de nødvendige komponenter som vi plejer
2. Indsæt {navigation} i parameter parentesen i stedet for props, så vi henter fra react navigations parametre som skal sendes videre fra forskellige Screens
   3. Hint: `function CarList = ({navigation}) => {}`
2. Opret en state for `cars, setCars` med useState
3. Lav nu en useEffect funktion, hvori der laves en if(!cars) og derefter indsætter kode fra hint 4
   1. Husk at useEffect kun skal køres en gang med et tomt array til sidst
4. Efter useEffect laves et if statement som kontrollerer, om der ikke er nogle biler og returner `<Text>Loadding...</Text> `
5. Lav nu en funktion kaldet `const handleSelectCar = id => {}`, som søger direkte i vores array af biler og finder bilobjektet, som matcher idet vi har tilsendt og ligger det ned i vores parametre i navigation (hint 5)
6. Over return funktionen her skal vi have to const variabler carArray med værdier (Object.values()), og carKeys med Obejct.keys()` const carArray = Object.values(cars); const carKeys = Object.keys(cars);`
7. I Return funktionen vil vi lave en `<FlatList/>`, hvori der skal laves følgende tre attributter
   1. data={carArray}
   2. keyExtractor med en funktion med parametrene item, index som sættes med carKeys[index] (``keyExtractor={(item, index) => carKeys[index]}``)
   3. renderItem med følgende return funktion: ``({item,index}) => { return() } ``. I return funktionen skal vi have et parent `TouchableOpacity` med en onpress funktion: `handleSelectCar(carKeys[index])`
      1. lav et Text element med `{item.brand}`
      2. Hint ``` <TouchableOpacity style={styles.container} onPress={() => handleSelectCar(carKeys[index])}> ```
9. Nu skulle du gerne kunne trykke på bil-modellen, du har oprettet i tidligere step og gå til car details

### CarDetails
1. Impoter de nødvendige komponenter som vi plejer
   2. Hint: `import {useEffect, useState} from "react";
      import { getDatabase, ref, remove } from "firebase/database";`
2. Start med at indsætte {navigation,route} i stedet for props ved CarDetails præcis som i de andre komponenter
3. Opret en state for cars med useState
4. Lav nu en useEffect, og hent car values og sæt dem med setCar(route.params.car[1]). Når vi forlader screenen, skal objektet tømmes: `return () => { setCar({}) }`
   5. Hint: ``` useEffect(() => {
        setCar(route.params.car[1])
        return () => {
            setCar({})
        };
    }, []);```
5. Opret nu en funktion kaldet handleEdit, som henter car objektet fra ``route.params.car``, og sæt det i navigation, så vi navigerer til 'Edit Car' - hint: kig tilbage i navigationsøvelsen
   1. Hint: `` const handleEdit = () => {
      // Vi navigerer videre til EditCar skærmen og sender bilen videre med
      const car = route.params.car
      navigation.navigate('Edit Car', { car });
      }; ``
6. Lav nu en confirmDelete funktion hvor du laver en Alert.alert med inspiration fra hint 6
7. Lav nu en handleDelete funktion, som først henter id fra route med ``route.params.car[0]`` og lav en try catch, hvor i try'en kaldes næsten den samme firebase funktion som du har hentet "cars" med tidligere, men til sidst tilføj `.remove()` og i ref'en ændres `/Cars/${id}`. I catchen, skal parametren tage `error`som argument og inde i catchen laves en Alert med error.message
   1. Hint 8
8. Nedenfor laves nu et lignende if statemtent: hvis der ikke er en car, skal vi returnere et text element med vilkårlig tekst
9. I return funktionen laves to knapper, som kalder på hhv handleEdit(), og confirmDelete()
10. Prøv at forstå hint 7, og indsæt derefter knapper
11. Nu burde du se alle bilens informationer og kunne slette din bil. Og du burde kunne gå til edit/add screen


### Add_edit_Car | step 2
1.Nu vil vi gerne have, at når vi kommer fra CarDetails, så tager den vores bil med og indsætter værdierne i felterne
2. Opret nu en useEffect med en funktion
   1. Her i denne funktion skal der først oprettes et if statement, med isEditCar
   2. I if funktionen, så skal du hente din bils objekt fra paramentrene `route.params.car[1]` og setNewCar(car)
   3. Dernæst lav en return med en funktion, som siger setNewCar(initalState)for at fjerne data, når vi går væk fra screenen
   4. Husk et tomt array til sidst i useEffecten
3. Dernæst i handleSave lav et if else statement
   1. i if'en brug isEditCar, og deri lav en try catch som tager udgangspunkt i følgende https://firebase.google.com/docs/database/web/read-and-write 
   2. dernæst i else'en lig add funktionen 
4. Gå nu ned i titlen på button nederst og sæt et isEditCar ? "Save changes" : "Add car" i titlen
5. done
Hint:
```
const [newCar,setNewCar] = useState(initialState);

    /*Returnere true, hvis vi er på edit car*/
    const isEditCar = route.name === "Edit Car";

    useEffect(() => {
        if(isEditCar){
            const car = route.params.car[1];
            setNewCar(car)
        }
        /*Fjern data, når vi går væk fra screenen*/
        return () => {
            setNewCar(initialState)
        };
    }, []);

    const changeTextInput = (name,event) => {
        setNewCar({...newCar, [name]: event});
    }

    const handleSave = async () => {

        const { brand, model, year, licensePlate } = newCar;

        if(brand.length === 0 || model.length === 0 || year.length === 0 || licensePlate.length === 0 ){
            return Alert.alert('Et af felterne er tomme!');
        }

        if(isEditCar){
            const id = route.params.car[0];
            // Define the path to the specific car node you want to update
            const carRef = ref(db, `Cars/${id}`);

            // Define the fields you want to update
            const updatedFields = {
                brand,
                model,
                year,
                licensePlate,
            };
            
            // Use the 'update' function to update the specified fields
            await update(carRef, updatedFields)
                .then(() => {
                Alert.alert("Din info er nu opdateret");
                const car = newCar
                navigation.navigate("Car Details", { car });
                })
                .catch((error) => {
                console.error(`Error: ${error.message}`);
                });

        }else{
        // Define the path to the "Cars" node where you want to push the new data
        const carsRef = ref(db, "/Cars/");
        
        // Data to push
        const newCarData = {
            brand,
            model,
            year,
            licensePlate,
        };
        
        // Push the new data to the "Cars" node
        await push(carsRef, newCarData)
            .then(() => {
            Alert.alert("Saved");
            setNewCar(initialState);
            })
            .catch((error) => {
            console.error(`Error: ${error.message}`);
            });
    }

```


## Hints 
### Skabelon 1
``` 
const KomponentNavn = (props) => { 
    return (
        <Text>Det er mit KomponentNavn</Text>
    )
}

export default KomponentNavn; 
```

### powermove JS funktion 2 ( husk turborgklammer når vi er i return )
```
Object.keys(initialState).map((key,index) =>{
   return(
       <View style={styles.row} key={index}>
           <Text style={styles.label}>{key}</Text>
           <TextInput
               value={newCar[key]}
               onChangeText={(event) => changeTextInput(key,event)}
               style={styles.input}
           />
       </View>
   )
})
```

### hint 3
```
 try {
    firebase
        .database()
        .ref('/Cars/')
        .push({ brand, model, year, licensePlate });
    Alert.alert(`Saved`);
    setNewCar(initialState)
} catch (error) {
    console.log(`Error: ${error.message}`);
}
```

### hint 4
```
useEffect(() => {
        const db = getDatabase();
        const carsRef = ref(db, "Cars");
    
        // Use the 'onValue' function to listen for changes in the 'Cars' node
        onValue(carsRef, (snapshot) => {
            const data = snapshot.val();
            if (data) {
                // If data exists, set it in the 'cars' state
                setCars(data);
            }
        });
    
        // Clean up the listener when the component unmounts
        return () => {
            // Unsubscribe the listener
            off(carsRef);
        };
    }, []); // The empty dependency array means this effect runs only once

    // Vi viser ingenting hvis der ikke er data
    if (!cars) {
        return <Text>Loading...</Text>;
    }
```
### hint 5
```
   const handleSelectCar = id => {
        /*Her søger vi direkte i vores array af biler og finder bil objektet som matcher idet vi har tilsendt*/
        const car = Object.entries(cars).find( car => car[0] === id /*id*/)
        navigation.navigate('Car Details', { car });
    };
```

### hint 6
```
if(Platform.OS ==='ios' || Platform.OS ==='android'){
            Alert.alert('Are you sure?', 'Do you want to delete the car?', [
                { text: 'Cancel', style: 'cancel' },
                // Vi bruger this.handleDelete som eventHandler til onPress
                { text: 'Delete', style: 'destructive', onPress: () => handleDelete() },
            ]);
        }
```

### hint 7 
```
Object.entries(car).map((item,index)=>{
                    return(
                        <View style={styles.row} key={index}>
                            {/*Vores car keys navn*/}
                            <Text style={styles.label}>{item[0]} </Text>
                            {/*Vores car values navne */}
                            <Text style={styles.value}>{item[1]}</Text>
                        </View>
                    )
                })
```

### Hint 8

```
const handleDelete = async () => {
        const id = route.params.car[0];
        const db = getDatabase();
        // Define the path to the specific car node you want to remove
        const carRef = ref(db, `Cars/${id}`);
        
        // Use the 'remove' function to delete the car node
       await remove(carRef)
            .then(() => {
                navigation.goBack();
            })
            .catch((error) => {
                Alert.alert(error.message);
            });
    };
```


## referencer
https://reactnavigation.org/docs/stack-navigator/
https://reactnavigation.org/docs/bottom-tab-navigator/
UseEffect:https://reactjs.org/docs/hooks-effect.html 
