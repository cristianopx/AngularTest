# Evaluación de conocimientos en Angular

## Objetivo
Evaluar el nivel de conocimiento de los participantes en Angular, desde conceptos básicos hasta temas avanzados.

## Estructura
La evaluación se divide en tres categorías: básico, intermedio y avanzado. Cada categoría contiene ejercicios prácticos, preguntas estructuradas y soluciones detalladas.

## Categorías

### Básico

#### Ejercicios prácticos:
1. Crear un componente básico en Angular con HTML, CSS y TypeScript.

```ts
// archivo: mi-componente.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-mi-componente',
  templateUrl: './mi-componente.component.html',
  styleUrls: ['./mi-componente.component.css']
})
export class MiComponenteComponent {
  titulo = 'Mi Primer Componente';
}
```
```html
<!-- archivo: mi-componente.component.html -->
<h1>{{ titulo }}</h1>
```
```css
/* archivo: mi-componente.component.css */
h1 {
  color: blue;
  font-size: 24px;
}
```

2. Enlazar datos de una propiedad a la vista utilizando la técnica de interpolación.

```ts
// archivo: datos.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-datos',
  template: `
    <h2>{{ nombre }}</h2>
    <p>Edad: {{ edad }}</p>
    <p>Ciudad: {{ ciudad }}</p>
  `
})
export class DatosComponent {
  nombre = 'Juan Pérez';
  edad = 30;
  ciudad = 'Madrid';
}
```

3. Manejar eventos de clic en elementos de la interfaz de usuario.
```ts
// archivo: contador.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-contador',
  template: `
    <h2>Contador: {{ contador }}</h2>
    <button (click)="incrementar()">Incrementar</button>
    <button (click)="decrementar()">Decrementar</button>
  `
})
export class ContadorComponent {
  contador = 0;

  incrementar() {
    this.contador++;
  }

  decrementar() {
    this.contador--;
  }
}
```

#### Preguntas estructuradas:
1. ¿Qué es Angular y cuáles son sus principales características?
2. ¿Qué son los componentes en Angular y cómo se utilizan?
3. ¿Qué son las directivas en Angular y cuáles son sus tipos?

### Intermedio

#### Ejercicios prácticos:
1. Crear un servicio en Angular para consumir datos de una API externa.
```ts
// archivo: api.service.ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class ApiService {
  private apiUrl = 'https://api.ejemplo.com/datos';

  constructor(private http: HttpClient) {}

  obtenerDatos(): Observable<any> {
    return this.http.get(this.apiUrl);
  }
}
```

2. Implementar formularios reactivos con validaciones utilizando FormGroup y FormControl.
```ts
// archivo: formulario.component.ts
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-formulario',
  template: `
    <form [formGroup]="formulario" (ngSubmit)="onSubmit()">
      <input formControlName="nombre" placeholder="Nombre">
      <input formControlName="email" placeholder="Email">
      <button type="submit" [disabled]="!formulario.valid">Enviar</button>
    </form>
  `
})
export class FormularioComponent implements OnInit {
  formulario: FormGroup;

  constructor(private fb: FormBuilder) {}

  ngOnInit() {
    this.formulario = this.fb.group({
      nombre: ['', [Validators.required, Validators.minLength(3)]],
      email: ['', [Validators.required, Validators.email]]
    });
  }

  onSubmit() {
    if (this.formulario.valid) {
      console.log(this.formulario.value);
    }
  }
}
```

3. Crear un componente con directivas personalizadas para modificar el comportamiento de elementos HTML.
```ts
// archivo: resaltar.directive.ts
import { Directive, ElementRef, HostListener } from '@angular/core';

@Directive({
  selector: '[appResaltar]'
})
export class ResaltarDirective {
  constructor(private el: ElementRef) {}

  @HostListener('mouseenter') onMouseEnter() {
    this.resaltar('yellow');
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.resaltar(null);
  }

  private resaltar(color: string | null) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}
```
```ts
// archivo: componente-con-directiva.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-componente-con-directiva',
  template: `
    <p appResaltar>Este texto se resaltará al pasar el mouse</p>
  `
})
export class ComponenteConDirectivaComponent {}
```

#### Preguntas estructuradas:
1. ¿Qué es el patrón MVC y cómo se aplica en Angular?
2. ¿Qué son las tuberías en Angular y cómo se utilizan?
3. ¿Cómo se implementa el enrutamiento de páginas en una aplicación Angular?

### Avanzado (40%)

#### Ejercicios prácticos:
1. Implementar una estrategia de manejo de errores en una aplicación Angular utilizando el servicio HttpClient.
```ts
// archivo: error-handling.service.ts
import { Injectable } from '@angular/core';
import { HttpErrorResponse } from '@angular/common/http';
import { throwError } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class ErrorHandlingService {
  handleError(error: HttpErrorResponse) {
    let errorMessage = '';
    if (error.error instanceof ErrorEvent) {
      // Error del lado del cliente
      errorMessage = `Error: ${error.error.message}`;
    } else {
      // Error del lado del servidor
      errorMessage = `Código de error: ${error.status}\nMensaje: ${error.message}`;
    }
    console.error(errorMessage);
    return throwError(() => new Error(errorMessage));
  }
}
```
```ts
// archivo: data.service.ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable, catchError } from 'rxjs';
import { ErrorHandlingService } from './error-handling.service';

@Injectable({
  providedIn: 'root'
})
export class DataService {
  private apiUrl = 'https://api.ejemplo.com/datos';

  constructor(
    private http: HttpClient,
    private errorHandler: ErrorHandlingService
  ) {}

  obtenerDatos(): Observable<any> {
    return this.http.get(this.apiUrl).pipe(
      catchError(this.errorHandler.handleError)
    );
  }
}
```

2. Implementar la comunicación entre componentes utilizando un servicio.
```ts
// archivo: comunicacion.service.ts
import { Injectable } from '@angular/core';
import { BehaviorSubject } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class ComunicacionService {
  private mensajeSource = new BehaviorSubject<string>('mensaje inicial');
  mensajeActual = this.mensajeSource.asObservable();

  cambiarMensaje(mensaje: string) {
    this.mensajeSource.next(mensaje);
  }
}

```
```ts
// archivo: componente-uno.component.ts
import { Component } from '@angular/core';
import { ComunicacionService } from './comunicacion.service';

@Component({
  selector: 'app-componente-uno',
  template: `
    <div>
      <h2>Componente Uno</h2>
      <button (click)="nuevoMensaje()">Nuevo Mensaje</button>
    </div>
  `
})
export class ComponenteUnoComponent {
  constructor(private comunicacionService: ComunicacionService) {}

  nuevoMensaje() {
    this.comunicacionService.cambiarMensaje('Mensaje desde Componente Uno');
  }
}

```
```ts
// archivo: componente-dos.component.ts
import { Component, OnInit } from '@angular/core';
import { ComunicacionService } from './comunicacion.service';

@Component({
  selector: 'app-componente-dos',
  template: `
    <div>
      <h2>Componente Dos</h2>
      <p>{{mensaje}}</p>
    </div>
  `
})
export class ComponenteDosComponent implements OnInit {
  mensaje: string;

  constructor(private comunicacionService: ComunicacionService) {}

  ngOnInit() {
    this.comunicacionService.mensajeActual.subscribe(mensaje => this.mensaje = mensaje);
  }
}

```

3. Optimizar el rendimiento de una aplicación Angular utilizando técnicas como lazy loading ,change detection y signal.
```ts
// archivo: app-routing.module.ts
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

const routes: Routes = [
  {
    path: 'feature',
    loadChildren: () => import('./feature/feature.module').then(m => m.FeatureModule)
  }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}
```
```ts
// archivo: performance.component.ts
import { Component, ChangeDetectionStrategy, signal } from '@angular/core';

@Component({
  selector: 'app-performance',
  template: `
    <h2>{{ title }}</h2>
    <ul>
      <li *ngFor="let item of items()">{{ item }}</li>
    </ul>
    <button (click)="addItem()">Add Item</button>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class PerformanceComponent {
  title = 'Lista de elementos';
  items = signal(['Item 1', 'Item 2', 'Item 3']);

  addItem() {
    this.items.update(items => [...items, `Item ${items.length + 1}`]);
  }
}
```

#### Preguntas estructuradas:
1. ¿Qué es la inyección de dependencias y cómo se utiliza en Angular?
2. ¿Cómo se implementa el testing unitario de componentes en Angular?
3. ¿Cómo se maneja el estado global en una aplicación Angular?
