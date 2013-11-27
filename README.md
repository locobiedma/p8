Plataphorma
===========

Meteor pet project created to teach my students the following Meteor functionality: 

* Collection's publish/subscribe 
* Deps.autorun 
* Meteor.methods/call 
* Integration of non-Meteor code in compatibility folder (HTML5 games Alien Invasion and Froot Wars)
* Usage of allow to control client access to collections

![ScreenShot](/screenshot.png)


Plataphorma offers the possibility to run 2 different HTML5 games: Alien Invasion and Froot Wars. 

On the right side of the screen the best players of each game are shown, updated in real time each time a signed in player finishes a game. If no game is selected, the best players overall are shown.

On the left side of the screen a chatroom for the current game is available. Only signed in users can post messages. If no game is selected a general chatroom is shown.

The original code of the two HTML5 games integrated in this project is available here:
* Alien Invasion: https://github.com/cykod/AlienInvasion
* Froot Wars: http://www.wrox.com/WileyCDA/WroxTitle/Professional-HTML5-Mobile-Game-Development.productCd-1118301323,descCd-DOWNLOAD.html

Bootstrap style (file bootstrap.min.css) provided by http://bootswatch.com


Running the project
-------------------

A live version of this code is running here: http://plataphorma.meteor.com

To run the project locally, clone the repo and run ```meteor``` inside it. You can see in .meteor/packages that this Meteor project uses these packages:
* ```meteor remove autopublish```
* ```meteor remove insecure```
* ```meteor add bootstrap```
* ```meteor add accounts-ui```
* ```meteor add accounts-password```


1) Click en botón de juego

Mediante el evento "Template.choose_game.events", cuando se hace click sobre uno de los botones, el que se ha pinchado se muestra y el que no se ha pinchado se oculta. Estamos hablando del fichero client/client.js

Template.choose_game.events = {
    'click #AlienInvasion': function () {
	$('#gamecontainer').hide();
	$('#container').show();
	var game = Games.findOne({name:"AlienInvasion"});
	Session.set("current_game", game._id);
    },
    'click #FrootWars': function () {
	$('#container').hide();
	$('#gamecontainer').show();
	var game = Games.findOne({name:"FrootWars"});
	Session.set("current_game", game._id);
    },
    'click #none': function () {
	$('#container').hide();
	$('#gamecontainer').hide();
	Session.set("current_game", "none");
    }
}

2) Se escribe mensaje chat sin estar autenticado

En el fichero client/client.js, cuando se activa el evento sobre el identificador #message, (caja para escribir texto) se comprueba si se pulsa la tecla "enter" (tecla número 13), en ese momento, con la línea if (Meteor.userId()) comprueba si el usuario está autenticado o no. Si no lo está, se muestra el evento con identificador "login-error" definido en index.html en la línea 60 con el que se muestra el mensaje  "You must be signed in to post messages."


3) Se escribe mensaje chat estando autenticado

En el fichero client/client.js, cuando se activa el evento sobre el identificador #message, (caja para escribir texto) se comprueba si se pulsa la tecla "enter" (tecla número 13), en ese momento, con la línea if (Meteor.userId()) comprueba si el usuario está autenticado o no. Si está autenticado guarda el mensaje, el id_usuario, la fecha actual y el game_id en su base de datos "Messages". 


4) Se termina partida con puntuación alta sin estar autenticado

En el lado del servidor, mediante el método matchFinish, mediante la línea "if (this.userId)" se comprueba si el usuario está autenticado o no, y si no lo está no se hace nada. Nos referiamos a la línea 46 del fichero server.js.
Está función es llamada desde el fichero game.js cuando se gana un juego y cuando se pierde.


5) Se termina partida con puntuación alta estando autenticado

En el lado del servidor, mediante el método matchFinish, mediante la línea "if (this.userId)" se comprueba si el usuario está autenticado o no, y si lo está se guardá en la base de datos "Matches" que está definida en el fichero models.js la hora actual, los puntos y el identificador del juego. Nos referiamos a la línea 46 del fichero server.js. Está función del servidor es llamada desde game.js cuando se gana un juego y cuando se pierde.



6) Qué sale en la consola cuando te autenticas (sig in)

En el lado del cliente, salta la función mostrada a continuación.

var currentUser = null;
Deps.autorun(function(){
    console.log("current user: " + currentUser);
    currentUser = Meteor.userId();
    console.log("current user: " + currentUser);
});


7) Qué sale en la consola cuando sales (sign out)

Desde el lado del cliente se ejecuta:

var currentUser = null;
Deps.autorun(function(){
    console.log("current user: " + currentUser);
    currentUser = Meteor.userId();
    console.log("current user: " + currentUser);
});

el id del usuario se pone a null y se extrae la base de datos dicho identificador, que ahora será igual a null.
En la consola del navegador se imprimirá dicho id del usuario.









