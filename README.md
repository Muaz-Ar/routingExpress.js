als ersten 
npm init -y für eine initialisierung oder erstellung von package.json 
danach installieren wir express 

---- npm i express 

wir verwenden hier noch zusätzlich ein tool namens nodemon was automatisch unser server aktuallisiert wenn 
wir etwas verändern sonst müssen wir diesen schritt manuell durchführen 
--save-dev es gibt sass paket als Entwicklungsabhängigkeit in pacjage.json datei hinzu 

--- npm i --save-dev nodemon 

in package.json definieren wir noch ein befehl welche datei von nodemon bei aktuallisierung verändert werden soll. Unter 
  "scripts": {
    "devSt": "nodemon server.js" das in meinem falll 
    {"heir befehlname"}: {"nodemon {heir welches js-datei}"}
  },

  mit  --- npm run devST   starten wir dann die devumgebung  

jetzt erstellen wir in der server.js unseren expressserver. 
mit  ----- const express = require('express')    importieren wir unsere expressbibliothek von 
npm. 
als nächstens erstellen wir  ---- const app = express() beim aufrufen von app wird express initialisiert

mit der methode listen() starten wir den server und fügen dann auch die Portnummer hinzu 

----- app.listen(3000)

also wier haben dann 4 verschiedene hauptruten 
app.get (read) app.post (Create) app.put (update) app.delete (delete)

der aufbau ist überall glei 

also app.get('{hier der pfad von http}', {hier kommt eine funkrion}) 

der aufbau der funktion ist wie folgt (hier haben wir 3 parameter req, res, "next") => {
res.send('willkommen')      --- clien side .send fürs testen doch wird nicht oft verwendet
console.log('hallo')        --- serversite bei jedem aufruf von "/"
res.sentStatus(500)         ---client kann es sehen wird oft verwendet hier z.b. error auf dem server
res.Status(500).send("HI")  -- status sieht man auf der console hi nachricht auf dem port
res.Status(500).json("message":"Error") -- wenn man json sendet könnte es eine api sein um nachrichten
                                         an meine clien zu schicken 
res.json("message":"Error") -- nur eine json senden 
res.download("server.js")   --- z.b wenn er dann auf den port kommt wird diese datei direkt download




} 
wobei nur (res) und (req) zu 99% verwendet werden 

app.get('./', (reg, res, next) => {

})
wir können auch res.render('index') so kann man index daten in expresss rendern 
dafür erstellt man eine index.ejs datei mit html code !
dann fügen wir zu unserem server app.set('view engine', 'ejs') hinzu bei der abfrage senden wier daann 
ein res.render('index')

durch verwenden von <%= locals.text %> in unserer index.ejs können wir daten von unserem server.js aus der z.b. app.get('/', (reg, res, ) => {
    console.log("hallo")
    res.render("index", {text:"World"}) }) auslesen. 


    < ------------- routing Router-------------> 
wir erstellen eine aplication in unserer app das heist: in unserer server.js haben wir unsere hautprout und wir erstellen unterrouts in anderen foldern. 
in der regel sollte man dann einen ordner mit routes erstellen und dann nehmen wir an wir wollen das routing von users machen. 

dafür erstellen eine datei users.js 

-- const express = require("express"); 
-- const router = express.Router();      -- express.Router() macht das selbe wie app 

router.get("/", (req, res) =>{
    res.send("user List")
})

router.get("/new", (req, res) =>{
    res.send("user List")
})

module.exports = router   // wir müssen alles exportieren damit man es auf der hauptseite nutzen kann 

dann füget man noch in der server.js folgendes hinzu: 

const userRouter = require("./routes/users")  // hier definieren wir userRouter und sagen das es die   
                                                daten von dem pfile importieren soll 
app.use("/users", userRouter)    mit app.use sagen wir das unsere daten von (routes/users) also die userRouter den pfard /users nutzen sollen. 

const express = require("express"); 
const router = express.Router(); 

router.get("/", (req, res) =>{
    res.send("user List")
})

router.get("/new", (req, res) =>{
    res.send("user List")
})

router.post("/", (req, res) =>{     hier geben wir den befehl zur erstellung eines neuen users 
    res.send("Create User")
})

router.get("/:id", (req, res) =>{                    hier holenwir die infos über eine id 
    res.send(`Get User With ID ${req.params.id}`)    dynamische abfrage 
})
                                                        wie man bemerkt sind alle 3 abragen identisch 
router.put("/:id", (req, res) =>{                       bis auf die tatsache das es .put .get .delete 
    res.send(`Update User With ID ${req.params.id}`)       sind wir können alle zusammenfassen mit 
})

router.delete("/:id", (req, res) =>{
    res.send(`DElete User With ID ${req.params.id}`)
})


router.route("/id")                         //mit route haben wir alle zusamengefasst 
.get((req, res) =>{                     
    res.send(`Get User With ID ${req.params.id}`)    
})
.put((req, res) => {
    res.send(`Update User With ID ${req.params.id}`)
})
.delete((req, res) => {
    res.send(`delete User With ID ${req.params.id}`)
})
const users = [{name: "kyle"}, {name: "sally"}]

router.param("id", (req, res, next, id)=>{   mit router.param das ist ein midelway es sucht den 
    req.user=users[id]              parameter über die id welches der user eingibt und
                            aktualliesiert. es handelt sich hierbei um eine abfrage 
    next()               
})


eine weitere middelware funkiton um etwas auszuloggen in unserem fall die URL dazu muss man in der server.js diesen code eingeben und dann noch der app sagen das sie dies nutzen soll um konflikte zu meiden alle console log entfärnen 
app.use(logger)
function logger(req, res, next) {
    console.log(req.originalUrl)
}
man kann auch logger direkt in eine anfrage schreiben z.b app.get('/', logger, (reg, res, ) => {

    res.render("index", {text:"World"})
})
dann wir es nur dann gelogt wenn ich auf home seite bin anders nicht. mann kann es auch oft verwenden 

middelwere zur nutzung von statischen datein wie html-css javascript 
mit dem befehl 
--- app.use(express.static("public")) 

verwendet express alle file in dem ordner public was man erstellt und man hat zugriff mit der filesystem was man dann auto erstellt wird

mit dem html befehl in combination von aktion und der form werden durch betätigen eines buttons die befehle an einen server gesendet und danach ausgelesen 

router.post("/", (req, res) =>{
  const isValid = true
  if (isValid) {
    users.push({firstName: req.body.firstName}) # mit push wird der import zu user hinzugefügt
    res.redirect(`/users/${users.length - 1}`) lenght -1 da bei 0 ein objekt beginnt 
  }else {      redirect in eingabe wird in einer anderen seite angezeigt 
    console.log("Error")
    res.render("users/new", {firstName: req.body.firstName})
  }
})

für eine queryabfrage benutzen wir req.quary.name(); .value etc. 

mit app.use(express.json()) kann express json lesen 