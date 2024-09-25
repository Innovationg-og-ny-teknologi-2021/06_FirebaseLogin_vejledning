# Øvelsesvejldning øvelse 6 - Firebase Authentication & Login ᕦ(ò_óˇ)ᕤ

## Slutprodukt

https://user-images.githubusercontent.com/48329669/128593607-d93036b5-331f-4c92-a17d-27c7b92244b9.mp4



## Integration med Firebase
[comment]: <> (This is a comment, it will not be included)

1.   Start med at oprette et nyt projekt som vi har gjort mange gange før. 
2.   Installér følgende dependencies;
    - firebase
    - react-native-paper
        - Et skærmklip af package.json filen til den endelige løsning er vedlagt i bilag A.
          Din package.json bør være ens (undtagen version nr.) med denne efter installeringen. 
        - KOPIER linjen herunder til installering.


        `npx expo install firebase react-native-paper`

3. Opret nu en authentication-database i Firebase;
- Følg dette link: https://firebase.google.com/
- Tryk på "Get Started"
- Tryk på "Add Project"
- Giv projektet et vilkårligt navn og tryk "continue"
- Fjern Analytics og tryk "create project"
- Registrer nu en  web Aapplikation. Tryk på ikonet </> - Se billeder herunder;
![img_1](https://user-images.githubusercontent.com/55731954/128145049-ee6b0029-1372-45d6-b6ec-0fefaf49e18b.png)
  
- Giv applikationen et vilkårligt navn og tryk "Register app".
- Du vil nu få præsenteret en kodeblok. Kopiér den del, som er omkranset af en rød boks på billedet herunder;
!<img width="586" alt="Skærmbillede 2022-09-19 kl  17 08 22" src="https://user-images.githubusercontent.com/111279752/191050825-a7a61a96-9e08-4cba-9aae-22e1b2cba631.png">

  
- Tryk på "Authentication"
- Tryk på "Get Started"
- Aktiver sign-in ved brug af Email/Password
<br/>
  
<Br><Br/>

# App.js (°ロ°)☝

I din IDE(Webtstorm, VC, Phpstorm, el.lign.)
- Indsæt den kode, som er kopieret fra firebase i App.js under jeres imports ligesom vi gjorde sidst
- Husk også at import ``import { getApps, initializeApp } from "firebase/app";``

Opret nu 2 mapper i dit project: 
- `components` Som skal have 2 components - `LogInComponent.js` & `SignUpComponent.js`
- `screens` som skal have 2 screens - `AuthScreen.js` & `MainScreen.js`

Indsæt følgende kode på din AuthScreen.js: 
```javascript
import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View, SafeAreaView } from 'react-native';

export default function AuthScreen() {
  return (
    <SafeAreaView style={styles.container}>
        <View style={styles.componentsBox}>
            <Text>Her skal vi have SignUp<Text/> 
        </View>
        <View style={styles.componentsBox}>
            <Text>Her skal vi have Login<Text/> 
        </View>
      <StatusBar style="auto" />
    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
  componentsBox: {
    width: '90%',
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
    margin: 20,
    borderRadius: 10,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.25,
  },
});
```
Importere denne screen til din App.js og se om det virker

<Br> </Br>

# SignUpComponent ( ͡o ͜ʖ ͡o)


1. importer `firebase` således: ``import { getAuth, createUserWithEmailAndPassword } from "firebase/auth";``
   - I skal huske at impotere de moduler fra firebase i bruger. I dette tilfælde er det `getAuth` og `createUserWithEmailAndPassword`
2. Importér `useState` fra react.
   - Dokumentatoinen på useState kan findes på følgende link:<br/> https://reactjs.org/docs/hooks-state.html
3. Opret const til; 
   1. email, 
   2. password 
   3. ``const [isCompleted, setCompleted] = useState(false)``
   4. ``const [errorMessage, setErrorMessage] = useState(null) ``
   5. ``const auth = getAuth();``
   
    <br/>Eksempel på syntaksen for én af variablerne: `const [email, setEmail] = useState('')`


3. i `return()` oprettes en overskrift og to inputfelter, som skal tage imod email og password samt en `<Button/>`der står for aktivering af en brugeroprettelse.
    - Husk at lav jeres button
    - Huske at i kan skal indramme jeres inputfelter felterne i et `<TextInput />`
=======
HINT: Brug løbende den officielle dokumentation:<br/> https://docs.expo.io/guides/using-firebase/ <br/>

```Javascript
 const renderButton = () => {
        return <Button onPress={() => handleSubmit()} title="Create user" />;
    };

return (
        <View>
            <Text style={styles.header}>Sign up</Text>
            <TextInput
                placeholder="email"
                value={?}
            />
            <TextInput
                placeholder="password"
                value={?}
            />
            {errorMessage && (
                <Text style={styles.error}>Error: {errorMessage}</Text>
            )}
            {renderButton()}
        </View>
    );
```

5. Opret nu en metode `handleSubmit`, som har til formål at aktivere en brugeroprettelse i firebase. 
      - HINT: https://firebase.google.com/docs/auth/web/password-auth.  
      - HUSK: Input felterne skal i `onChangeText` dynamisk sætte værdierne for email & password
```Javascript
    /*
   * Metoden herunder håndterer oprettelse af brugere ved at anvende den prædefinerede metode, som stilles til rådighed af firebase
   * createUserWithEmailAndPassword tager en mail og et password med som argumenter og foretager et asynkront kald, der eksekverer en brugeroprettelse i firebase https://firebase.google.com/docs/auth/web/password-auth#create_a_password-based_account
   * Opstår der fejl under forsøget på oprettelse, vil der i catch blive fremsat en fejlbesked, som, ved brug af
   * setErrorMessage, angiver værdien for state-variablen, errormessage
   */
      const handleSubmit = async() => {
        await createUserWithEmailAndPassword(auth, email, password)
        .then((userCredential) => {
          // Signed in 
          const user = userCredential.user;
          // ...
        })
        .catch((error) => {
          const errorCode = error.code;
          const errorMessage = error.message;
          setErrorMessage(errorMessage)
          // ..
        });
      }
```


6. Importér `SignUpForm` i jeres App.js og placér komponenten i `return()`
7. Sørg for at din kode afprøver om en firebase app allerede er initialiseret, før selve initialiseringen opstår
   - HINT: https://dev.to/lxnd77/comment/13fbb
   - Hint: Billag B
   - Husk at importer getApps fra firebase/app
8. Start nu appen og opret en testbruger. F.eks.<br/>E-mail: test@mail.dk<br/> password: 1234
- Gå ind Firebase -> Authentication -> Users og se se at din bruger nu er oprettet i dit projekt. 

<Br><Br/>

# LogInComponent.js ( ° ͜ʖ °)

1. `LogInComponent.js` er kodemæssigt næsten nøjagtig ens med `SignUpComponent.js`. Kopiér derfor al kode fra `SignUpComponent` og placér dette i `LogInComponent.js` 
    - HUSK: du skal justere komponentens titel og ændre de firebase componenter i importerer. 
3. Justér teksten på `<Button/>` fra at være "create user" til "login"
4. Dernæst skal knappen aktivere en loginmetode fremfor en sign-in metode.
- HINT: Firebase stiller en prædefineret metode til rådighed ligesom med signIn metoden:<br/> https://firebase.google.com/docs/auth/web/password-auth

<Br><Br/>

# MainScreen (ง ͡ʘ ͜ʖ ͡ʘ)ง

1. I din `return`, Opret en `<Text/>`, der udskriver e-mailen på den bruger, som er logget ind. 
    -  HINT: Led efter en metode i dokumentationen, som henter oplysninger på en aktiv bruger. 
    

2.  Opret en log ud `Button`.
    -   HINT: Firebase stiller en log-ud metode til rådighed Find metoden i dokumentationen.
    -  HINT: 
    ```JavaScript
    const auth = getAuth();
    const user = auth.currentUser
    
    // handleLogout håndterer log ud af en aktiv bruger.
    // Metoden er en prædefineret metode, som firebase stiller tilrådighed  https://firebase.google.com/docs/auth/web/password-auth#next_steps
    // Metoden er et asynkrontkald.
    
    const handleLogOut = async () => {
        await signOut(auth).then(() => {
            // Sign-out successful.
          }).catch((error) => {
            // An error happened.
          });
    };
    ```

<Br><Br/>

# App.js ᕦ( ᴼ ڡ ᴼ )ᕤ

Tilbage i din App.js, skal vi nu indsætte følgende under din `export default function App() {` 
```Javascript
const [user, setUser] = useState({ loggedIn: false });
  

  if (getApps().length < 1) {
    initializeApp(firebaseConfig);
    console.log("Firebase On!");
    // Initialize other firebase products here
  };

  const auth = getAuth();
```

Her opretter vi en user state hvis inital state er logget ud. Vi checker også hvis firebase er tændt og opretter en auth constant som vi henter fra `getAuth()`

Vi skal nu lave en function der gør det muligt for brugeren at ændre denne state ved at login. 

## Firebase onAuthStateChanged

Funktionen `onAuthStateChanged` bruges til at observere ændringer i brugerens login-status. Vi vil bruge denne funktion til at holde styr på, om en bruger er logget ind eller logget ud.

### Opgave:

1. Opret en funktion kaldet `onAuthStateChange`, som modtager en `callback`.
2. Brug `onAuthStateChanged` til at lytte efter ændringer i autentificeringsstatus fra Firebase.

Her er en generel opbygning af funktionen:

```javascript
function onAuthStateChange(callback) {
    return onAuthStateChanged(auth, (user) => {
        if (user) {
            // Brugeren er logget ind
            const uid = user.uid;
            callback({ loggedIn: true, user: user });
            console.log("You are logged in!");
        } else {
            // Brugeren er logget ud
            callback({ loggedIn: false });
        }
    });
}
```

## Implementering af onAuthStateChanged med useEffect

Bruge `useEffect`-hooken til dynamisk at observere, om en bruger er logget ind eller ej ved hjælp af Firebase's `onAuthStateChanged`.

## Forstå `useEffect`

React's `useEffect`-hook bruges til at køre sideeffekter i funktionelle komponenter. I dette tilfælde vil vi bruge `useEffect` til at observere ændringer i brugerens autentificeringsstatus.

- **useEffect** kører en funktion, når komponenten monteres, og kan også returnere en funktion, som køres, når komponenten afmonteres.

### useEffect:

1. Opret en `useEffect`-hook for at lytte til brugerens login-status ved hjælp af Firebase's `onAuthStateChanged`.
2. Brug `onAuthStateChange`-funktionen til at aktivere lytteren og opdatere brugerens status.
3. Returner en `unsubscribe`-funktion for at afmelde lytteren, når komponenten afmonteres.

## TImplementering af `useEffect`

Her er et exemple på, hvordan du implementerer det:

```javascript
useEffect(() => {
    // Aktivér vores lytter, der observerer ændringer i brugerens autentificeringsstatus
    const unsubscribe = onAuthStateChange(setUser);

    // Returner en funktion for at afmelde lytteren, når komponenten afmonteres
    return () => {
      unsubscribe();
    };
}, []);
```
Forklaring af `useEffect(() => { ... }, []);`

- useEffect kører koden indenfor () første gang komponenten renders. Den tomme array [] sikrer, at effekten kun kører én gang, når komponenten monteres.

Til sidst skal vi i din `return` kalde på det overstående. 
```javascript
  return (
   user.loggedIn ? <MainScreen/> : <AuthScreen />
  );
```

### Nu skulle din app gerne virke! (•__•) ( •_•)>⌐■-■ (⌐■_■)

## Bilag

### Bilag A - Package.json - Fra Endelig Løsning - Jeres versions er ikke det samme <br/>
<img width="344" alt="Skærmbillede 2022-09-19 kl  17 03 10" src="https://user-images.githubusercontent.com/111279752/191049330-5fa21a5f-6f5f-4fc6-a887-7699851eeab8.png">

### Bilag B - 
```javascript
  if (getApps().length < 1) {
    initializeApp(firebaseConfig);
    console.log("Firebase On!");
    // Initialize other firebase products here
  } else {
    console.log("Firebase not on!");
  }

```