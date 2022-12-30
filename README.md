# Gustoso Gourmet Front-end
En este documento se podrá visualizar parte del código y funcionamiento del mismo para la creación visual de paquetes de Gustoso Gourmet.

## Índice
- [Creación de vista](https://github.com/valeuwu/Front-paquetes#creaci%C3%B3n-de-vista)
  * [Cálculo de disponibilidad](https://github.com/valeuwu/Front-paquetes#c%C3%A1lculo-de-disponibilidad)
    - [Script Disponibilidad](https://github.com/valeuwu/Front-paquetes#script-disponibilidad)
  * [Stepper](https://github.com/valeuwu/Front-paquetes#stepper)
    - [Script Stepper](https://github.com/valeuwu/Front-paquetes#script-stepper)
    - [Botones del usuario (Para función cambiarPaso(n))](https://github.com/valeuwu/Front-paquetes#botones-del-usuario-para-funci%C3%B3n-cambiarpason)
    - [Script slider de productos](https://github.com/valeuwu/Front-paquetes#script-slider-de-productos)
    - [Nombre del producto bajo su imagen](https://github.com/valeuwu/Front-paquetes#nombre-del-producto-bajo-su-imagen)
  * [Lista de productos seleccionados](https://github.com/valeuwu/Front-paquetes#lista-de-productos-seleccionados)
    - [Script de lista de productos](https://github.com/valeuwu/Front-paquetes#script-de-lista-de-productos)
  * [Cambio de valor final del Paquete](https://github.com/valeuwu/Front-paquetes#cambio-de-valor-final-del-paquete)
    - [Script para calcular precio final](https://github.com/valeuwu/Front-paquetes#script-para-calcular-precio-final)
    
# Creación de vista
## Cálculo de disponibilidad

Esta sección debe ir en la parte superior del código antes del comienzo del formulario. En esta se representa la cantidad de paquetes que puede comprar el cliente dependiendo del valor mínimo de productos existentes a escoger.

```liquid
<!--Calculo de disponibilidad-->
  {% if product.stock > 0 %}
    <div class=" form-group {% if product.status == 'available' %}visible{% else %}hidden{% endif %}">
      <div id="stock" style="">
        <label class="form-control-label" style="font-weight: 700;">Disponibilidad: </label>
        
        <span id="product-form-stock" class="product-form-stock"> Seleccione sus productos </span>
      </div>
      <br>
    </div>
    
  {% endif %}
```
### Script disponibilidad
Para realizar el cálculo se utilizó el siguiente script. En la sección bajo el título de "Cambios de disponibilidad", se encuentran variables, las cuales van en búsqueda del del stock disponible de cada producto que se escoja. Se necesita recorrer para obtener la información. Y por último se utiliza **Math.min** para comparar el stock de los productos, dejando como mínimo el menor valor de estos.

```liquid
<!--Aparición de nombre bajo el producto-->
<script>

for(let cont = 1; cont < 4; cont++){
    $('[name="mermeladas'+cont+'"]').on('change', function() {
    // Mostrar titulo debajo
    $(".titulo_m"+cont).hide();
    $("#label-" + this.id).show();
    $("#label-" + this.id).css('display', 'block');
    //
    
    $('#valor-mermelada').text("Mermelada: "+ this.value);
    
    <!--Calculo de disponibilidad-->
    var data_1 = document.querySelector('input[name="pastas"]:checked').getAttribute('id').split("-");
    var data_2 = document.querySelector('input[name="mermeladas1"]:checked').getAttribute('id').split("-");
    var data_3 = document.querySelector('input[name="mermeladas2"]:checked').getAttribute('id').split("-");
    var data_4 = document.querySelector('input[name="mermeladas4"]:checked').getAttribute('id').split("-");
    //var data_2 = this.id.split("-");
    var stock_1 = data_1[0];
    var stock_2 = parseInt(data_2[0].substring(1, data_2[0].length));
    var stock_3 = parseInt(data_3[0].substring(1, data_3[0].length));
    var stock_4 = parseInt(data_4[0].substring(1, data_4[0].length));

    var max_stock = Math.min(stock_1,stock_2,stock_3,stock_4)
    $('#product-form-stock').text(max_stock)
    
    var input = document.getElementById("input-qty");
    input.value = 1
    input.setAttribute("max",max_stock); // set a new value;
    
    });
}
```

## Stepper

Para visualizar cada ventana de pasos dentro del stepper, se debe crear un div con la clase **tab** para su correcto funcionamiento, dentro de esta sección se deberá incluir todo lo que se quiere mostrar dependiendo del paso que se esté tomando (El código que se muestra a continuación es del paquete 4). Se debe especificar el producto que se desea, como se puede ver se utilizó un **if** para recoger los productos de Molinillo, excluyendo la "Pimienta". En caso de que los productos sean más de 1, se debe incluir las **Flechas** que se encuentran incluidas como imagenes, que permitiran transcurrir los productos como slider. Dentro de este se encuentra el input que permitirá obtener la información del producto escogido, su nombre e imagen. 

```liquid
  <!--Molinillos de sal de mar o rosada-->
  <div class="tab" id="{{product.id}}">
          <label class="product-option__title form-control-label titulo-color " style="font-size: 16px;color: #363636;font-weight: 700;"> Selecciona un Molinillo: </label>
          <fieldset class="field-group colors prod-options">

          <div class="teste-slid"> <!--slider-->
              <img class="boton anterior" src="{{ 'left-arrow.png' | asset }}" id="anterio" style="width:40px; height:40px;">
                {% for product in products.all%}
                  {% for category in product.categories %}
                    
                    {% if category.id == 1358119 %} <!-- Id molinillos -->
                        {% assign nom_mol = product.name | split: " " | first %}
                        {% assign ult_nom = product.name | split: " " | last %}

                        {% if nom_mol == 'Molinillo' and ult_nom != 'Pimienta'%}
                            <div class="color-option mer" style="height: auto;">
                            <input
                                type="radio"
                                name="mermeladas"
                                value="{{product.name}}"
                                id="{{product.stock}}-{{product.id}}"
                                onclick="(mostrarvalor_mer(this))"
                            >
                            <label
                                for="{{product.stock}}-{{product.id}}"
                                name="mermeladas"
                                title="{{product.name}}"
                                style="background-image:url('{{ product.images.first | thumb: '100×100' }}');width:100px;height:100px;border-radius: 5px;border: none;"
                            ></label>
                            <!--<input id="label-{{product.stock}}-{{product.id}}" class="titulo_m marquee m_vi" style="font-size: 15px;width:100px;display:none;" value="{{product.name}}" readonly>-->
                            <p id="label-{{product.stock}}-{{product.id}}" class="titulo_m" style="overflow: auto;width: 100px;display:none; font-size: 13px;">{{product.name}}</p>
                            
                            </div>
                        {% endif %}
                    {% endif %}
                  {% endfor %}
                {% endfor %}
              <img class="boton siguiente" src="{{ 'right-arrow.png' | asset }}" id="siguient" style="width:40px; height:40px;">
          </div>
        </fieldset>
        </div>
```

### Script Stepper
Este script permite ir cambiando de plantilla dependiendo del indíce en el que se encuentre el indicador. La función **cambiarPaso(n)** se encarga de realizar el cálculo para lograr cambiar de paso, dependiendo del botón que presione el usuario. La función **pasoActual** permite mostrar el paso actual en la plantilla y a la vez mostrar que botones deben ir en cada plantilla.

```liquid
<script>
  var plantillaActual = 0; 
  pasoActual(plantillaActual); 
  function pasoActual(n) {
    var ubicacion = document.getElementsByClassName("tab");
    ubicacion[n].style.display = "block";
    if (n == 0) {
      document.getElementById("prevBtn").style.display = "none";
      document.getElementById("inicioBtn").style.display = "none";
      document.getElementById("nextBtn").style.display = "inline";
    }else if (n == (ubicacion.length-1)){
      document.getElementById("inicioBtn").style.display = "inline";
      document.getElementById("nextBtn").style.display = "none";
    }else {
      document.getElementById("prevBtn").style.display = "inline";
      document.getElementById("nextBtn").style.display = "inline";
      document.getElementById("inicioBtn").style.display = "none";
    }
    
  }

  function cambiarPaso(n) {
    var x = document.getElementsByClassName("tab");
    x[plantillaActual].style.display = "none";
    plantillaActual = plantillaActual + n;
    
    pasoActual(plantillaActual);
  }

</script>
```
#### Botones del usuario (Para función cambiarPaso(n))
Esta sección debe ubicarse antes del fin del formulario de los productos. Se consideran 3 botones, "prevBtn" que permite retroceder, "nextBtn" permite avanzar en el stepper e "inicioBtn" que permite dirigirse al primer paso dentro del formulario. Para que la dirección sea indicada correctamente para dirigirse al inicio, se debe calcular cuantos valores se deben retroceder desde el último paso para llegar a 0.

```liquid
<!--Boton de cambio de vista-->
  <div style="overflow:auto;">
    <div style="float:left;">     
      <button type="button" id="prevBtn" onclick="cambiarPaso(-1)" class="boton-tab boton-ante">Anterior</button>
      <button type="button" id="nextBtn" onclick="cambiarPaso(1)" class="boton-tab">Siguiente</button>
      <button type="button" id="inicioBtn" onclick="cambiarPaso(-6)" class="boton-tab">Inicio</button>
    </div>
  </div>
```

### Script slider de productos

Para los botones se han utilizado dos imagenes que representan flechas que actúan como botones de movimiento del slider. Para su funcionamiento se utilizó el siguiente script. Como se puede visualizar, la primera parte del código, bajo el for, toma por constantes (o variables) el nombre de la imagen por su id por cada uno de los pasos y se toma el slider como tal. Al botón se le asigna que este pendiente a un click y se moverá 110 pixeles, a la izquierda (-) y a la derecha (+). Considerando la existencia de 4 productos o menos, se ha creado la parte inferior del código, en el cual dependiendo del tamaño de la ventana, aparecerán o desaparecerán las flechas, esto siendo indicado en cada condicional del tamaño que se pueda tener en pixeles.

```liquid
<!--Movimiento de stepper y aparición de flechas según tamaño de ventana-->
<script>
    for(let btn_cont = 1; btn_cont < 3; btn_cont++){
        const ant_1 = document.querySelector('#anterio1');
        const ant_2 = document.querySelector('#anterio2');
        const sig_1 = document.querySelector('#siguient1');
        const sig_2 = document.querySelector('#siguient2');
        const slider = document.querySelector('.teste-slid'+btn_cont);

        ant_1.addEventListener('click', () => {
            slider.scrollLeft -= 110 //440
        })

        sig_1.addEventListener('click', () => {
            slider.scrollLeft += 110
        })

        ant_2.addEventListener('click', () => {
            slider.scrollLeft -= 110 //440
        })

        sig_2.addEventListener('click', () => {
            slider.scrollLeft += 110
        })

        var width = (window.innerWidth > 0) ? window.innerWidth : screen.width;
        if(($(".teste-slid"+btn_cont+" .mer").length > 4) || ($(".teste-slid"+btn_cont+" .mer").length > 4) && width < 1400 || ($(".teste-slid"+btn_cont+" .mer").length <= 4) && width < 768){
          document.getElementById("anterio"+btn_cont).style.display = "inline";
          document.getElementById("siguient"+btn_cont).style.display = "inline";
        }else if (($(".teste-slid"+btn_cont+" .mer").length <= 4) && width < 992 && width > 768 || ($(".teste-slid"+btn_cont+" .mer").length <= 4) && width > 1400){
          document.getElementById("anterio"+btn_cont).style.display = "none";
          document.getElementById("siguient"+btn_cont).style.display = "none";
        }
        
        var cuerpom = document.getElementsByTagName("BODY")[0];
        var widthm = cuerpom.offsetWidth;

        if (window.addEventListener) {  
          window.addEventListener ("resize", onResizeEvent, true);
        } else {
          if (window.attachEvent) {  
            window.attachEvent("onresize", onResizeEvent);
          }
        }

        function onResizeEvent() {
          bodyElement = document.getElementsByTagName("BODY")[0];
          newWidthm = bodyElement.offsetWidth;
          if(newWidthm != widthm){
            widthm = newWidthm;
            if(($(".teste-slid"+btn_cont+" .mer").length > 4 || width < 1400 && width > 992 && ($(".teste-slid"+btn_cont+" .mer").length <= 4) ||  (width < 768 && $(".teste-slid"+btn_cont+" .mer").length <= 4))){
              document.getElementById("anterio"+btn_cont).style.display = "inline";
              document.getElementById("siguient"+btn_cont).style.display = "inline";
            }else if(width > 1350 && $(".teste-slid"+btn_cont+" .mer").length <= 4 || ($(".teste-slid"+btn_cont+" .mer").length <= 4) && width < 900 && width > 670){
              document.getElementById("anterio"+btn_cont).style.display = "none";
              document.getElementById("siguient"+btn_cont).style.display = "none";
            }
          }
        }
};
</script>
```

### Nombre del producto bajo su imagen

Esto permite que cada vez que el usuario presione un producto aparecerá el nombre del mismo bajo su imagen, **label** como se indica dentro del stepper. ***Este script se encuentra junto al script de disponibilidad.***

```liquid
<!--Aparición de nombre bajo el producto-->
<script>

for(let cont = 1; cont < 4; cont++){
    $('[name="mermeladas'+cont+'"]').on('change', function() {
    // Mostrar titulo debajo
    $(".titulo_m"+cont).hide();
    $("#label-" + this.id).show();
    $("#label-" + this.id).css('display', 'block');
    //
```

## Lista de productos seleccionados

Esta sección le permite al usuario el poder visualizar los productos que escogió durante el proceso del paquete. Estos aparecen con la imagen del respectivo producto y su nombre.

```liquid
<!--Lista de productos seleccionados-->
<div class="lista-productos" style="height: auto; margin-top: 20px;">
  <label class="titulo-productos-seleccionados">Productos seleccionados:</label> 
    <div class="contenido">
      <div class="div_sp" id="vista_m">
        <img class="thumb_sp" id="thumb_m"/>
        <input class="input_product" type="text" id="result_m" value="" readonly> 
      </div>
      <div class="div_sp" id="vista_m2">
        <img class="thumb_sp" id="thumb_m2"/>
        <input class="input_product" type="text" id="result_m2" value="" readonly> 
      </div>
      <div class="div_sp" id="vista_m3">
        <img class="thumb_sp" id="thumb_m3"/>
        <input class="input_product" type="text" id="result_m3" value="" readonly> 
      </div>
      <div class="div_sp" id="vista_p">
        <img class="thumb_sp" id="thumb_p"/>
        <input class="input_product" type="text" id="result_p" value="" readonly>
      </div>
    </div>
    <br>
</div>
```

### Script de lista de productos
Estas funciones van en busqueda del producto input seleccionado, se recibe el nombre e imagen del mismo en una constante. Dependiendo del producto que se escoja este podrá mostrarse con un **display: flex;**.

```liquid
<!--Mostrar productos seleccionados en lista-->
<script>
  function mostrarvalor_mer(input_label){
    const mermeladas = input_label.value;
    const img_source_thumb = input_label.labels[0].style.backgroundImage.match(/\"(.*?)\"/)[1];
    //console.log(mermeladas);
    if(mermeladas.split("-")[1] == 1){
        var mermelada1 = mermeladas.split("-")[0];
        document.getElementById("result_m").value = mermelada1;
        document.getElementById("thumb_m").src = img_source_thumb;
        var m = document.getElementById("vista_m");
        m.style.display = "flex"; 
        //console.log(mermelada1)
        let precio = mermelada1 != 'Molinillo de Mix Pimienta' ? '{{product.price | price}}' : '{{product.price | plus:1000 | price}}'
        let precio_final_elem = document.querySelector(".product-heading__pricing").innerHTML = precio;

    }else if(mermeladas.split("-")[1] == 2){
        var mermelada2 = mermeladas.split("-")[0];
        document.getElementById("result_m2").value = mermelada2;
        document.getElementById("thumb_m2").src = img_source_thumb;
        var m2 = document.getElementById("vista_m2");
        m2.style.display = "flex";
    }else{
        var mermelada2 = mermeladas.split("-")[0];
        document.getElementById("result_m3").value = mermelada2;
        document.getElementById("thumb_m3").src = img_source_thumb;
        var m2 = document.getElementById("vista_m3");
        m2.style.display = "flex";
    }
  }


  function mostrarvalor_pas(pasta_label){
      const pastas = pasta_label.value;
      const img_source_thumb = pasta_label.labels[0].style.backgroundImage.match(/\"(.*?)\"/)[1];
          var pasta1 = pastas.split("-")[0];
          document.getElementById("result_p").value = pasta1;
          document.getElementById("thumb_p").src = img_source_thumb;
          var p = document.getElementById("vista_p");
          p.style.display = "flex";
  }

</script>
```
## Cambio de valor final del Paquete 

Para realizar este cambio se ha tenido que realizar un calculo de las variantes de productos que pueda escojer el usuario. Se han incluido dos variables dentro del parcial **product-pricing** que tomarán el precio calculado.

```liquid
<!-- Inputs para validar precio / deben estar invisible -->
<input class="variable_precio_1" type='hidden' value=''></input>
<input class="variable_precio_2" type='hidden' value=''></input>
```

### Script para calcular precio final

Esta parte de código se encuentra dentro de la función de lista de productos. Se realiza el cálculo de productos seleccionados dependiendo si estos son los que corresponden a las variables que agregan un precio extra como por ejemplo el "Molinillo de Mix Pimienta".

```liquid
<script>
let precio = mermelada1 != 'Molinillo de Mix Pimienta' ? '{{product.price | price}}' : '{{product.price | plus:1000 | price}}'
let precio_final_elem = document.querySelector(".product-heading__pricing").innerHTML = precio;
</script>
```

En caso de existir más variables que el "Molinillo de Mix Pimienta" se debe realizar de la siguiente manera.

```liquid
<script>
function validar_precio_final(){
    let variable_precio_1 = document.querySelector('.variable_precio_1').value
    let variable_precio_2 = document.querySelector('.variable_precio_2').value
    
    let array_var1 = variable_precio_1.split("");
    let array_var2 = variable_precio_2.split("");



    // Procesar valores finales
    let cant_var_1 = array_var1.length;
    let cant_var_2 = array_var2.length;


    let valor_total = parseInt("{{product.price}}") + (parseInt(cant_var_1) * 1000) + (parseInt(cant_var_2)*1500) // Valor base + la cantidad de variantes 1 + la cantidad de variantes 2
    let valor_final = new Intl.NumberFormat('es-CL', {currency: 'CLP', style: 'currency'}).format(valor_total) // transforma a string moneda
    document.querySelector(".product-heading__pricing").innerHTML = valor_final;
  }

  function agregar_variaciones(id_step){

    let var_precio_2 = document.querySelector('.variable_precio_2').value
    let array_var_2 = var_precio_2.split("")
    if(!array_var_2.includes(id_step)){ // si el valor no se encuentra en el array, entonces lo agrega
      document.querySelector('.variable_precio_2').value = var_precio_2 + id_step;
      //console.log(document.querySelector('.variable_precio_2').value);
    }
    // si el valor ya existe entonces no hace nada, ya que se entiende que seleccionó otra mermelai
  }

  function eliminar_variacion(id_step){
    let var_precio_2 = document.querySelector('.variable_precio_2').value
    let array_var_2 = var_precio_2.split("")
    if(array_var_2.includes(id_step)){
      document.querySelector('.variable_precio_2').value = var_precio_2.replace(id_step,'');
      //console.log(document.querySelector('.variable_precio_2').value);
    }
  }
 </script>
```

