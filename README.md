# Farmacia_Virtual
<!DOCTYPE html>
<html lang="es">
<head>
<title>Farmacia Virtual</title>
</head>
<body onLoad=startclock()>
<h1>Farmacia Virtual</h1>
    <meta charset="UTF-8">
    <title>Farmacia Virtual</title>
    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.0/css/bootstrap.min.css" integrity="sha384-9gVQ4dYFwwWSjIDZnLEWnxCjeSWFphJiwGPXr1jddIhOegiu1FwO5qRGvFXOdJZ4" crossorigin="anonymous">
    <script>
        window.onload = function () {
            // Variables
            let baseDeDatos = [
                {
                    id: 1,
                    nombre: 'Desloratadina Caja',
                    precio: 26.86,
                    imagen: 'desloratadina.jpg'
                },
                {
                    id: 2,
                    nombre: 'Loratadina Jarabe',
                    precio: 10.55,
                    imagen: 'loratadina jarabe.jpg'
                },
                {
                    id: 3,
                    nombre: 'Nor-Cetin Forte Caja',
                    precio: 43.28,
                    imagen: 'norcetin.jpg'
                },
                {
                    id: 4,
                    nombre: 'Cetirizina Caja',
                    precio: 6.15,
                    imagen: 'cetitizina.jpg'
                }

            ]
            let $items = document.querySelector('#items');
            let carrito = [];
            let total = 0;
            let $carrito = document.querySelector('#carrito');
            let $total = document.querySelector('#total');
            let $botonVaciar = document.querySelector('#boton-vaciar');

            // Funciones
            function renderItems() {
                for (let info of baseDeDatos) {
                    // Estructura
                    let miNodo = document.createElement('div');
                    miNodo.classList.add('card', 'col-sm-4');
                    // Body
                    let miNodoCardBody = document.createElement('div');
                    miNodoCardBody.classList.add('card-body');
                    // Titulo
                    let miNodoTitle = document.createElement('h5');
                    miNodoTitle.classList.add('card-title');
                    miNodoTitle.textContent = info['nombre'];
                    // Imagen
                    let miNodoImagen = document.createElement('img');
                    miNodoImagen.classList.add('img-fluid');
                    miNodoImagen.setAttribute('src', info['imagen']);
                    // Precio
                    let miNodoPrecio = document.createElement('p');
                    miNodoPrecio.classList.add('card-text');
                    miNodoPrecio.textContent = info['precio'] + '$';
                    // Boton 
                    let miNodoBoton = document.createElement('button');
                    miNodoBoton.classList.add('btn', 'btn-primary');
                    miNodoBoton.textContent = '+';
                    miNodoBoton.setAttribute('marcador', info['id']);
                    miNodoBoton.addEventListener('click', anyadirCarrito);
                    // Insertamos
                    miNodoCardBody.appendChild(miNodoImagen);
                    miNodoCardBody.appendChild(miNodoTitle);
                    miNodoCardBody.appendChild(miNodoPrecio);
                    miNodoCardBody.appendChild(miNodoBoton);
                    miNodo.appendChild(miNodoCardBody);
                    $items.appendChild(miNodo);
                }
            }

            function anyadirCarrito () {
                // Anyadimos el Nodo a nuestro carrito
                carrito.push(this.getAttribute('marcador'))
                // Calculo el total
                calcularTotal();
                // Renderizamos el carrito 
                renderizarCarrito();
            }

            function renderizarCarrito() {
                // Vaciamos todo el html
                $carrito.textContent = '';
                // Quitamos los duplicados
                let carritoSinDuplicados = [...new Set(carrito)];
                // Generamos los Nodos a partir de carrito
                carritoSinDuplicados.forEach(function (item, indice) {
                    // Obtenemos el item que necesitamos de la variable base de datos
                    let miItem = baseDeDatos.filter(function(itemBaseDatos) {
                        return itemBaseDatos['id'] == item;
                    });
                    // Cuenta el número de veces que se repite el producto
                    let numeroUnidadesItem = carrito.reduce(function (total, itemId) {
                        return itemId === item ? total += 1 : total;
                    }, 0);
                    // Creamos el nodo del item del carrito
                    let miNodo = document.createElement('li');
                    miNodo.classList.add('list-group-item', 'text-right', 'mx-2');
                    miNodo.textContent = `${numeroUnidadesItem} x ${miItem[0]['nombre']} - ${miItem[0]['precio']}$`;
                    // Boton de borrar
                    let miBoton = document.createElement('button');
                    miBoton.classList.add('btn', 'btn-danger', 'mx-5');
                    miBoton.textContent = 'X';
                    miBoton.style.marginLeft = '1rem';
                    miBoton.setAttribute('item', item);
                    miBoton.addEventListener('click', borrarItemCarrito);
                    // Mezclamos nodos
                    miNodo.appendChild(miBoton);
                    $carrito.appendChild(miNodo);
                })
            }

            function borrarItemCarrito() {
                console.log()
                // Obtenemos el producto ID que hay en el boton pulsado
                let id = this.getAttribute('item');
                // Borramos todos los productos
                carrito = carrito.filter(function (carritoId) {
                    return carritoId !== id;
                });
                // volvemos a renderizar
                renderizarCarrito();
                // Calculamos de nuevo el precio
                calcularTotal();
            }

            function calcularTotal() {
                // Limpiamos precio anterior
                total = 0;
                // Recorremos el array del carrito
                for (let item of carrito) {
                    // De cada elemento obtenemos su precio
                    let miItem = baseDeDatos.filter(function(itemBaseDatos) {
                        return itemBaseDatos['id'] == item;
                    });
                    total = total + miItem[0]['precio'];
                }
                // Formateamos el total para que solo tenga dos decimales
                let totalDosDecimales = total.toFixed(2);
                // Renderizamos el precio en el HTML
                $total.textContent = totalDosDecimales;
            }

            function vaciarCarrito() {
                // Limpiamos los productos guardados
                carrito = [];
                // Renderizamos los cambios
                renderizarCarrito();
                calcularTotal();
            }

            // Eventos
            $botonVaciar.addEventListener('click', vaciarCarrito);

            // Inicio
            renderItems();
        } 
          
    </script>

</div>
    <div class="container">
        <div class="row">
            <!-- Elementos generados a partir del JSON -->
            <main id="items" class="col-sm-8 row"></main>
            <!-- Carrito -->
            <aside class="col-sm-4">
                <h2>Carrito</h2>
                <!-- Elementos del carrito -->
                <ul id="carrito" class="list-group"></ul>
                <hr>
                <!-- Precio total -->
                <p class="text-right">Total: <span id="total"></span>$</p>
                <button id="boton-vaciar" class="btn btn-danger">Vaciar</button>
                 <form name="formulario" method="post" action="mailto:octa1829056@gmail.com">
                     <ul>
                    <li>Nombre: <input type="text" name="Nombre"/></li>

                    <li>Apellido: <input type="text" name="Apellido"/></li>

                    <li>Celular: <input type="text" name="Celular"/></li>

                    <li>Direccion: <input type="text" name="Direccion"></li>

                    <li>Jubilad@: <input type="checkbox" name="retired"/></li>

                    <li><p class="text-right">Total: <span id="#total"></span>$</p></li>
                    <li><input type="submit" value="Comprar"/></li>
                    <li><input type="reset" value="Borrar Informacion"/></li>
                </ul>
            </form>

            </aside>
        </div>
    </div>


<head><title>Calendario  </title>
</HEAD>

<BODY><body onLoad=startclock()>

<HTML>

<CENTER>
<font face="Times New Roman" size="5">


<b>Calendario</b></font><font face=arial size="1"><p>
<CENTER>
<script language="JavaScript">

<!-- Hide the script from old browsers --
var timerID = null;
var timerRunning = false;

function stopclock() {  
        if(timerRunning)    
        clearTimeout(timerID);  
        timerRunning = false;
}

function startclock() {  
        stopclock();  
        showtime();
}

function showtime () {
        var now = new Date();        
        var hours = now.getHours();        
        var minutes = now.getMinutes();        
        var seconds = now.getSeconds()        
        var timeValue = "" + ((hours >12) ? hours -12 :hours)        
        timeValue += ((minutes < 10) ? ":0" : ":") + minutes        
        timeValue += ((seconds < 10) ? ":0" : ":") + seconds        
        timeValue += (hours >= 12) ? " P.M." : " A.M."        
        document.clock.face.value = timeValue;        
        // you could replace the above with this        
        // and have a clock on the status bar:        
        // window.status = timeValue;        
        timerID = setTimeout("showtime()",1000);        
        timerRunning = true;
}
//  -->

</SCRIPT>

<SCRIPT LANGUAGE="JavaScript">

<!--  to hide script contents from old browsers

function greeting(){   
        var today = new Date();   
        var hrs = today.getHours();   
        document.writeln("<CENTER>");   
        document.write("<H1>Buen@s ");   
        if (hrs < 6)      document.write("(y tempranos) días");   
        else if (hrs < 12)      document.write("días");   
        else if (hrs <= 19)      document.write("tardes");   
        else      document.write("Noches");   document.writeln("!</H1>");   
        document.writeln("</CENTER>");   
        document.writeln("<FORM NAME='clock' onSubmit='0'>");   
        document.writeln("<DIV ALIGN=CENTER>");   
        document.writeln("<INPUT TYPE='text' NAME='face' SIZE=14 VALUE=''>");   
        document.writeln("</DIV>");   
        document.writeln("<FONT SIZE+=4>");
}

function montharr(m0, m1, m2, m3, m4, m5, m6, m7, m8, m9, m10, m11){   
        this[0] = m0;   
        this[1] = m1;   
        this[2] = m2;   
        this[3] = m3;   
        this[4] = m4;   
        this[5] = m5;   
        this[6] = m6;   
        this[7] = m7;   
        this[8] = m8;   
        this[9] = m9;   
        this[10] = m10;   
        this[11] = m11;
}

function calendar(){   
        var monthNames = "EneFebMarAbrMayJunJulAgoSepOctNovDec";   
        var today = new Date();   
        var thisDay;   
        var monthDays = new montharr(31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31);      
        year = today.getYear();   
        thisDay = today.getDate();      
        // leap year calculation   
        if (((year % 4 == 0) && (year % 100 != 0)) || (year % 400 == 0))      monthDays[1] = 29;
        // figure out how many days this month will have...   
        nDays = monthDays[today.getMonth()];   
        // and go back to the first day of the month...   
        firstDay = today;   
        firstDay.setDate(1);   
        // and figure out which day of the week it hits...   
        startDay = firstDay.getDay();        
        document.writeln("<CENTER>");   
        document.write("<TABLE BORDER>");   
        document.write("<TR><TH COLSPAN=7>");   
        document.write(monthNames.substring(today.getMonth() * 3,(today.getMonth() + 1) * 3));   
        document.write(". ");   
        document.write(year);   
        document.write("<TR><TH>");   
        document.write("Dom<TH>Lun<TH>Mar<TH>Mié<TH>Jue<TH>Vie<TH>Sáb");      
        // now write the blanks at the beginning of the calendar   
        document.write("<TR>");   column = 0;   for (i=0; i<startDay; i++)   {
              document.write("<TD><FONT SIZE+=4>");      
              column++;      
                document.write("</FONT>");   
}

   for (i=1; i<=nDays; i++)   {      
        document.write("<TD>");      
        if (i == thisDay)         
        document.write("<FONT COLOR=\"#FF0000\" SIZE+=4>")      
        document.write(i);      
        if (i == thisDay)        
                document.write("</FONT>")      
                column++;      
        if (column == 7)      {         
                document.write("<TR>"); 
                // start a new row         
                column = 0;      
        }   
   }   

document.write("</TABLE>");   
document.writeln("</CENTER>");
}
document.write(greeting());
//document.write("<HR>");
document.write(calendar());
document.write("</FONT>");
//document.write("<HR>");
// --End Hiding Here -->

</script>
<p>

<CENTER><P>
    </td></tr>
</table>
</BODY>
</HTML>
<h2>Acerca de Nosotros</h2>
<TABLE BORDER>
    <TR><Th>Intagram</Th>
        <TD>FarmaciaVirtual_Anton</TD>
    <TR><TH>Twitter</TH>
        <TD>@farmaciaVirtual_anton</TD>
    <TR><<TH>Geolocalizacion</TH>
        <TD>Avenida 5ta intercepcion con Calle 3ra</TD>
    <TR><TH>Telefono</TH>
        <TD>987-2518</TD>
</body>
</html>
