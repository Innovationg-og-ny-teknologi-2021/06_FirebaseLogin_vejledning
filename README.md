# 5_firebase-database_vejledning

## Slut produkt
https://user-images.githubusercontent.com/48329669/128408720-8449ae85-8722-4f91-8dc7-0fe58191fb75.mp4

## Integration med Firebase
1. Start med at oprette et nyt projekt.
2. Installér følgende dependencies med `expo install eller npm install`;
   1. ` expo install react-native-gesture-handler @react-native-community/masked-view react-native-reanimated react-native-safe-area-context react-native-screens @react-navigation/native @react-navigation/bottom-tabs @react-navigation/stack react-native-vector-icons firebase`
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
Da firebase blev opdateret her i slut August 2022, er der stadig en masse bugs som skaber problemer. Et af dem er følgende
`ERROR MESSAGE: "While trying to resolve module 'idb'..... Indeed none of these files exist":`

Denne fejl vil i muligvis få, når i forsøger at compile jeres applikation på jeres mobil. 

Læs om løsning hertil her: https://stackoverflow.com/questions/72179070/react-native-bundling-failure-error-message-while-trying-to-resolve-module-i

## App.JS - Opret React navigation
1. Efter din const af firebase Configarationen, skal Firebase initialiseres:
   1. ``` 
      if (!firebase.apps.length) {
      firebase.initializeApp(firebaseConfig);
      }
    ```
2. Initialiser Stack navigatoren med `const Stack = createStackNavigator();`
3. Lav derefter en funktion kaldet StackNavigation, som skal returnere 3 Screens med "name" samt komponentnavnene CarList, CarDetails og Add_edit_car
   1. Det er vigtigt at CarList placeres øverst ;)
4. Oppe ved `const Stack = createStackNavigator()`, Initialiser deraf en Bottom navigatorer med `const Tab = createBottomTabNavigator();`
5. Gå nu til return, og ligesom i de sidste navigation øvelser, så opret en ``<NavigationContainer></NavigationContainer>``, som skal wrappe din Tab.navigator --> Se https://reactnavigation.org/docs/bottom-tab-navigator/ 
6. I Tab.Navigator wrapper oprettes to Tab.Screen med name og component, som henholdvis skal være StackNavigation og Add_edit_Car
7. Tilføj et ikon til Home Tab.Screen | Eksempel ( husk at Ionicons skal importeres)
``<Tab.Screen name={'Home'} component={StackNavigation} options={{tabBarIcon: () => ( <Ionicons name="home" size={20} />),headerShown:null}}/>``
8. Tilføj nu også et ikon til "Add" Screenen, hvor ikon navnet er ``add`` istedet for home
9. Nu burde du kunne trykke imellem add og stacknavigatoren "Carlist"
10. Tips: Hvis du får fejl, så læs hvad react native brokker sig over, og kopier det ind i Google og brug stackOverFlow

## Add_edit_car komponent 
1. I parameter parantesen indsæt nu `{navigation,route}` i stedet for `props`, så vi  henter fra react navigations parametre som skal sendes videre fra forskellige Screens
2. Lav øverst en initialState object med tilhørende string attributter: brand,model,year og licensplate
   1. ``const initialState = {
      brand: '',
      model: '',
      year: '',
      licensePlate: ''
      }``
3. **States** |Lav nedenunder en ny State kaldet `[newCar,setNewCar]`, hvor du i useState bruger `initialState`
4. Lav derunder en const funktion kaldet isEditCar: `const isEditCar = route.name === "Edit car"` eller hvad du nu har kaldt dette screen name i stacknavigatoren i app.js
   1. ( bruges til senere )
5. Opret nu en changeTextInput funktion
   ``` 
   const changeTextInput = (name,event) => {
        setNewCar({...newCar, [name]: event});
   }
   ```
6. **Content |** Gå return og lav et SafeAreaView som parent, og deri et ScrollView
   1. Nu laver vi et über powermove med JS, hvor vi i vores return funktion skal returnere en række felter som vi skal kunne indtaste og ændre værdier i
      1. Start med i ScrollView'et at åbne eksikveringen af JS med {}, og deri lav et Object keys funktion af newCar, som har en funktion med parametrene for attributterne model osv og index. 
      2.  Se eventuelt nede i hints  `powermove JS funktion 2`
      3. Opret et return og Opret først et `View` med styling, deri et `Text` komponent som deri tager attributterne navnet 
      4. Derefter opret et `TextInput` som i value'en har en newCar[parameternavnet] og lav en attribute med on funktion som taget event og et kald changeSelect funktion som taget newCar parameter navn og event med som parameter
   2. lav nu en button som har en save changes funktion kaldt handlesave(), og giv en titel Add car
7. **HandleSave** | Lav en handleSave funktion, som først henter newCar objektets keys brand, model, year `const {brand,model .... etc } = newCar;`
   1. Opret et if, som kigger på om brand og de andre attributter har en længde på 0 for at tjekke for tomme felter fx `brand.length === 0` , og return en `Alert.alert()` med en besked
   2. Kopier koden fra hints 3 og placer den nedenunder. Prøv at forstå hvad der sker i funktionen 
8. Nå du trykke på button i din app burde du nu få en alert med at du har saved. Og derefter gå nu ind i på firebase (console.firebase.com) om din bil er blevet oprettet


### CarList
1. Start med at sætte I parameter parantesen indsæt nu `{navigation}` i props ved Carlist
2. Opret en state for `cars, setCars` med useState
3. Lav nu en useEffect funktion (se referncer hvis du har no clue, hvordan det fungere) hvori der laves en !cars, så kør hint 4
   1. Husk at useEffect kun skal køres en gang med et tomt array til sidst
4. Efter useEffect lav nu et if statement som kontrollere om der ikke er nogle biler og returner `<Text>Loadding...</Text> `
5. Lav nu en funktion kaldt `const handleSelectCar = id => {}`, som søger direkte i vores array af biler og finder bil objektet som matcher idet vi har tilsendt og ligger det ned i vores parametre i navigation ( hint 5)
6. Over return funktionen her skal vi have to const variabler carArray med værdier (Object.values()), og carKeys med Obejct.keys()` const carArray = Object.values(cars); const carKeys = Object.keys(cars);`
7. I Return funktionen vil vi lave en `<FlatList/>`, i FlatListen komponenten lav følgende tre attributter
   1. data som tager et med carArray
   2. keyExtractor med en funktion med parametrene item, index som sættes med carKeys[index] (``keyExtractor={(item, index) => carKeys[index]}``)
   3. renderItem med følgende return funktion: ``({item,index}) => { return() } ``. I return funktion skal vi have et forældre `TouchableOpacity` med en onpress funktion: `handleSelectCar(carKeys[index])`
      1. lav en Text element med `{item.brand}`
9. Nu skulle du gerne kunne trykke på bil modellen du har oprettet i tidligere step og går til car details

### CarDetails
1. Start med at sætte I parameter parantesen indsæt nu {navigation,route} i props ved CarDetails
2. Opret en state for cars med useState
3. Sæt nu en useEffect op, og Hent car values og sætter dem med setCar(route.params.car[1]), og når vi forlader screen, tøm object. Husk, at du ikke skal have tomt array i dependecies []
4. Opret nu en funktion kaldt handle edit, som henter car objektet  fra ``route.params.car``, og sætter det i navigation lige vi har gjort tidligere, men blot med et andet navn som skal være vores edit screen ( string)
5. Lav nu en confirmDelete funktion hvor du laver en Alert.alert med inspiration fra hint 6
6. Lav nu en handleDelete funktion, som først henter id fra route med ``route.params.car[0]`` og lav en try catch, hvor i try'en kaldes en næsten den samme firebase funktion som du har henter "cars" med tidligere, men tilsidst remove() og i ref'en ændres `/Cars/${id}`. I catchen set i parametren error og inde i catchen lav en Alert med error.message
7. inden return funktione lav nu en lignende if statemtent hvis der ikke er en car, returner Text element med vilkårlig tekst
8. I return funktionen lav to knapper som kalder på hhv handleEdit(), og confirmDelete()
9. Prøv at forstå hint 7, og indsæt efter knapper
10. Nu burde du se alle din bils informationer og kunne slette din bil. Og du burde kunne gå til edit/add screene


### Add_edit_Car | step 2
1.Nu vil vi gerne have at når vi kommer fra CarDetails, så tager den vores bil med og indsætterne værdierne i felterne
2. Opret nu en useEffect med en funktion
   1. Her i denne funktion skal der først oprettes et if statement, med isEditCar
   2. I if funktion, så skal du hente din bils objekt fra paramentrene `route.params.car[1]` og setNewCar(car)
   3. Dernæst lav en return med en funktion, som siger setNewCar(initalState)
   4. Husk et tomt array til sidst i useEffecten
3. Dernæst i handleSave lav et if else statement
   1. i if'en brug isEditCar, og deri lav en try catch som tager udgangspunkt i følgende https://firebase.google.com/docs/database/web/read-and-write 
   2. dernæst i else'en lig add funktionen 
4. Gå nu ned i titlen på button nederst og sæt et isEditCar ? "Save changes" : "Add car" i titlen
5. done


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

### powermove JS funktion 2 ( husk carlsberg klammer når vi er i return )
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
firebase
    .database()
    .ref('/Cars')
    .on('value', snapshot => {
            setCars(snapshot.val())
    });
```
### hint 5
```
const car = Object.entries(cars).find( car => car[0] === id /*id*/)
navigation.navigate('Car Details', { car });
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

## referencer
https://reactnavigation.org/docs/stack-navigator/
https://reactnavigation.org/docs/bottom-tab-navigator/
UseEffect:https://reactjs.org/docs/hooks-effect.html 
