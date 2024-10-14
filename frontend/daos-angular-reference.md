# Angular Reference

## Signal Query

Contiene los siguientes elementos:
- viewChild
- viewChildren
- contentChild
- contentChildren

## Animate CSS Grid

Mas información en: 
https://www.npmjs.com/package/animate-css-grid 

### Instalación

**Ejecutar** el siguiente command:

```
npm install animate-css-grid
```

### Configuración

En el archivo HTML:

- incluir en el `div` donde se aplica el `display: grid;`, ejemplo: 

``` html
<div #dashboard class="dashboard-widgets">
```

En el archivo TS:
- Agregar el import:

``` ts
import {wrapGrid} from "animate-css-grid";
```

- Agregar el atributo y Método:
``` ts
dashboard = viewChild.required<ElementRef>('dashboard');

ngOnInit() {
  wrapGrid(this.dashboard().nativeElement, {duration: 300});
}
```

## Animate Chart.js

Mas información en: 
https://www.npmjs.com/package/chart.js

### Instalación

**Ejecutar** el siguiente command:

```
npm install chart.js
```

### Configuración

En el archivo HTML:

- incluir en el `div` donde se aplica el `display: grid;`, ejemplo: 

``` html
<div #dashboard class="dashboard-widgets">
```

En el archivo TS:
- Agregar el import:

``` ts
import {wrapGrid} from "animate-css-grid";
```

- Agregar el atributo y Método:
``` ts
dashboard = viewChild.required<ElementRef>('dashboard');

ngOnInit() {
  wrapGrid(this.dashboard().nativeElement, {duration: 300});
}
```