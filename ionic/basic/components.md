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