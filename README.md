# Gustoso Gourmet Front-end
En este documento se podrá visualizar parte del código y funcionamiento del mismo para la creación visual de paquetes de Gustoso Gourmet.

##Índice
- Creación de vista
  * Cálculo de disponibilidad

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
