# Komponensek gyártása

Csináljuk egy összefoglaló komponenst:
`ionic g module components`

Ez csinál egy mappát `components` néven. Na most ebbe csináljunk pár komponenst amit használni szeretnénk majd:
```
ionic g component components/myComponent --export
ionic g component components/myComponent2 --export
ionic g component components/myComponent3 --export
ionic g component components/myComponent4 --export
```
A `components.module.ts` pedig így nézzen ki ezután:
```
...
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';
import { IonicModule } from '@ionic/angular';

import { MyComponentComponent } from './my-component/my-component.component';
import { MyComponentComponent2 } from './my-component2/my-component2.component';
import { MyComponentComponent3 } from './my-component3/my-component3.component';
import { MyComponentComponent4 } from './my-component4/my-component4.component';

const PAGES_COMPONENTS = [
  MyComponentComponent,
  MyComponentComponent2,
  MyComponentComponent3,
  MyComponentComponent4
];

@NgModule({
  imports: [
    CommonModule,
    FormsModule,
    IonicModule.forRoot(),
  ],
  declarations: [
    PAGES_COMPONENTS
  ],
  exports: [
    PAGES_COMPONENTS
  ],
  entryComponents: [],
})
export class ComponentsModule {}
```

Aztán importáljuk be a `ComponentsModule`-t az `app.module.ts`-ba:

```
...
import { ComponentsModule } from './components/components.module';
...

@NgModule({
  declarations: [AppComponent],
  entryComponents: [],
  imports: [
    ...
    ComponentsModule,
    ...
  ],
  providers: [
    ...
  ],
  bootstrap: [AppComponent]
})
export class AppModule {}
```
Hogy használjuk is, vegyünk egy meglévő oldalt, pl: `tab1.module.ts`:
```
...
import { ComponentsModule } from '../components/components.module';
...

@NgModule({
  imports: [
     ...
     ComponentsModule,
     ...
  ],
  declarations: [Tab1Page]
})
export class Tab1PageModule {}
```
Kész is a szenvedés része. Most pedig a HTML fájlba használjuk őket így:
```
<app-my-component></app-my-component>
<app-my-component2></app-my-component>
<app-my-component3></app-my-component>
<app-my-component4></app-my-component>
```
Ezeket a neveket innen derítheted ki/írhatod át: Nyisd meg a komponensed, pl: `my-component.component.ts`
Lesz benne egy ilyen rész:
```
@Component({
  selector: 'app-my-component',
  templateUrl: './my-component.component.html',
  styleUrls: ['./my-component.component.scss'],
})
```
Na a `selector:` rész lesz amit a html-be írsz.

### Változók belepakolása
A használt HTML fájlba ahol meghívod, nézzen ki így pl:
```
<app-kislabda
    value1="little cat"
    value2="big cat"
  ></app-kislabda>
```

Majd a `kislabda.component.ts` fájl pedig így:
```
import { Component, OnInit, Input } from '@angular/core';

@Component({
  selector: 'app-kislabda',
  templateUrl: './kislabda.component.html',
  styleUrls: ['./kislabda.component.scss'],
})
export class KislabdaComponent implements OnInit {

  @Input() value1: string;
  @Input() value2: string;

  constructor() {
    this.value1;
    this.value2;
  }

  ngOnInit() {}

}
```
Annyi hogy importálni kell az `Input`-ot felül, majd az export-ba pedig már látod mivan.

### Külső funkció meghívása templaten belül
Tegyük fel hogy neked van egy modalod amibe van egy template és abból akarod bezárni a modalt, itt a megoldás kéremszépen: (De amúgy ez érvényes bármire funkcióra)

Na van egy funkciód pl egy modal page-en ami felugrik ha meghívod és benne van a templated:
```
public async closeModal() {
  await this.modalController.dismiss();
}
```

A fenti a funkciód, a `multimodal.page.ts`-ben:
```
import { Component, OnInit } from '@angular/core';
import { ModalController } from '@ionic/angular';

@Component({
  selector: 'app-multimodal',
  templateUrl: './multimodal.page.html',
  styleUrls: ['./multimodal.page.scss'],
})
export class MultimodalPage implements OnInit {

  constructor(private modalController: ModalController) {
    this.closeModal();
  }

  ngOnInit() {
  }

  public async closeModal() {
    await this.modalController.dismiss();
  }

}
```

Ezután a fentiek alapján van egy komponensed, pl: `send.component.ts`, abba importálod azt amiből akarod kihasználni a funkciót:
```
import { MultimodalPage } from '../../multimodal/multimodal.page';
```

Azután a `export class SendComponent implements OnInit {}`-ba a `constructor` így nézzen ki:
```
constructor(private MultimodalPage: MultimodalPage) {
  this.MultimodalPage.closeModal();
}
```

Tehát adjuk hozzá itt a használni kívánt funkciót.
A HTML része a komponensnek amiből használjuk a másik funkciót az így nézzen ki meghívva:
```
<ion-button (click)="MultimodalPage.closeModal()">Gomba</ion-button>
```