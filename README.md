# Ion Range Calendar

[![NPM version][npm-image]][npm-url]
[![MIT License][license-image]][license-url]

Forked from : https://github.com/hsuanxyz/ion-range-calendar

* Supports date range.
* Supports multi date.
* Supports HTML components.
* Disable weekdays or weekends.
* Setting days event.
* Setting localization.
* Material design.
# Supports
* `"@ionic/angular": ">=4.0.0"`
# Usage

### Installation

```bash
 npm i @googlproxer/ion-range-calendar moment moment-timezone
 ```

### Import module

```typescript
import { NgModule } from '@angular/core';
import { IonicApp, IonicModule } from '@ionic/angular';
import { MyApp } from './app.component';
...
import { CalendarModule } from '@googlproxer/ion-range-calendar';

@NgModule({
  declarations: [
    MyApp,
    ...
  ],
  imports: [
    IonicModule.forRoot(),
    CalendarModule
  ],
  bootstrap: [MyApp],
  ...
})
export class AppModule {}
```

### Change Defaults

```typescript
import { NgModule } from '@angular/core';
import { IonicApp, IonicModule } from 'ionic-angular';
import { MyApp } from './app.component';
...
import { CalendarModule } from "ion-range-calendar";

@NgModule({
  declarations: [
    MyApp,
    ...
  ],
  imports: [
    IonicModule.forRoot(MyApp),
    // See CalendarComponentOptions for options
    CalendarModule.forRoot({
      doneLabel: 'Save',
      closeIcon: true
    })
  ],
  bootstrap: [IonicApp],
  entryComponents: [
    MyApp,
    ...
  ]
})
export class AppModule {}
```

# As Component

### Basic

```html
<ion-range-calendar [(ngModel)]="date" (change)="onChange($event)" [type]="type" [format]="'YYYY-MM-DD'">
</ion-range-calendar>
```

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'page-home',
  templateUrl: 'home.html'
})
export class HomePage {
  date: string;
  type: 'string'; // 'string' | 'js-date' | 'moment' | 'time' | 'object'
  constructor() { }

  onChange($event) {
    console.log($event);
  }
  ...
}
```

### Date range

```html
<ion-range-calendar [(ngModel)]="dateRange" [options]="optionsRange" [type]="type" [format]="'YYYY-MM-DD'">
</ion-range-calendar>
```

```typescript
import { Component } from '@angular/core';
import { CalendarComponentOptions } from '@googlproxer/ion-range-calendar';

@Component({
  selector: 'page-home',
  templateUrl: 'home.html'
})
export class HomePage {
  dateRange: { from: string; to: string; };
  type: 'string'; // 'string' | 'js-date' | 'moment' | 'time' | 'object'
  optionsRange: CalendarComponentOptions = {
    pickMode: 'range'
  };

  constructor() { }
  ...
}
```

### Multi Date

```html
<ion-range-calendar [(ngModel)]="dateMulti" [options]="optionsMulti" [type]="type" [format]="'YYYY-MM-DD'">
</ion-range-calendar>
```

```typescript
import { Component } from '@angular/core';
import { CalendarComponentOptions } from '@googlproxer/ion-range-calendar';

@Component({
  selector: 'page-home',
  templateUrl: 'home.html'
})
export class HomePage {
  dateMulti: string[];
  type: 'string'; // 'string' | 'js-date' | 'moment' | 'time' | 'object'
  optionsMulti: CalendarComponentOptions = {
    pickMode: 'multi'
  };

  constructor() { }
  ...
}
```

### Input Properties

| Name     | Type                     | Default      | Description  |
| -------- | ------------------------ | ------------ | ------------ |
| options  | CalendarComponentOptions | null         | options      |
| format   | string                   | 'YYYY-MM-DD' | value format |
| type     | string                   | 'string'     | value type   |
| readonly | boolean                  | false        | readonly     |

### Output Properties

| Name        | Type         | Description                |
| ----------- | ------------ | -------------------------- |
| change      | EventEmitter | event for model change     |
| monthChange | EventEmitter | event for month change     |
| select      | EventEmitter | event for day select |
| selectStart | EventEmitter | event for day select |
| selectEnd   | EventEmitter | event for day select |

### CalendarComponentOptions

| Name              | Type                    | Default                                                                                | Description                                       |
| ----------------- | ----------------------- | -------------------------------------------------------------------------------------- | ------------------------------------------------- |
| from              | Date                    | `new Date()` | start date                                        |
| to                | Date                    | 0 (Infinite)                                                                           | end date                                          |
| color             | string                  | `'primary'` | 'primary', 'secondary', 'danger', 'light', 'dark' |
| pickMode          | string                  | `single` | 'multi', 'range', 'single'                        |
| showToggleButtons | boolean                 | `true` | show toggle buttons                               |
| showMonthPicker   | boolean                 | `true` | show month picker                                 |
| monthPickerFormat | Array<string>           | `['JAN', 'FEB', 'MAR', 'APR', 'MAY', 'JUN', 'JUL', 'AUG', 'SEP', 'OCT', 'NOV', 'DEC']` | month picker format                               |
| defaultTitle      | string                  | ''                                                                                     | default title in days                             |
| defaultSubtitle   | string                  | ''                                                                                     | default subtitle in days                          |
| disableWeeks      | Array<number>           | `[]` | week to be disabled (0-6)                         |
| monthFormat       | string                  | `'MMM YYYY'` | month title format                                |
| weekdays          | Array<string>           | `['S', 'M', 'T', 'W', 'T', 'F', 'S']` | weeks text                                        |
| weekStart         | number                  | `0` (0 or 1)                                                                           | set week start day                                |
| daysConfig        | Array<**_DaysConfig_**> | `[]` | days configuration                                |
| maxRange          | number                  | 0 | The maximum range of the selection in days                        |

# As Modal

### Basic

Import ion-range-calendar in component controller.

```typescript
import { Component } from '@angular/core';
import { ModalController } from '@ionic/angular';
import {
  CalendarModal,
  CalendarModalOptions,
  DayConfig,
  CalendarResult
} from '@googlproxer/ion-range-calendar';

@Component({
  selector: 'page-home',
  templateUrl: 'home.html'
})
export class HomePage {
  constructor(public modalCtrl: ModalController) {}

  openCalendar() {
    const options: CalendarModalOptions = {
      title: 'BASIC'
    };

    const myCalendar = await this.modalCtrl.create({
      component: CalendarModal,
      componentProps: { options }
    });

    myCalendar.present();

    const event: any = await myCalendar.onDidDismiss();
    const date: CalendarResult = event.data;
    console.log(date);
  }
}
```

### Date range

Set pickMode to 'range'.

```typescript
openCalendar() {
  const options: CalendarModalOptions = {
    pickMode: 'range',
    title: 'RANGE'
  };

  const myCalendar = await this.modalCtrl.create({
    component: CalendarModal,
    componentProps: { options }
  });

  myCalendar.present();

  const event: any = await myCalendar.onDidDismiss();
  const date = event.data;
  const from: CalendarResult = date.from;
  const to: CalendarResult = date.to;

  console.log(date, from, to);
}
```

### Multi Date

Set pickMode to 'multi'.

```typescript
openCalendar() {
  const options = {
    pickMode: 'multi',
    title: 'MULTI'
  };

  const myCalendar = await this.modalCtrl.create({
    component: CalendarModal,
    componentProps: { options }
  });

  myCalendar.present();

  const event: any = await myCalendar.onDidDismiss();
  const date: CalendarResult = event.data;
  console.log(date);
}
```

### Disable weeks

Use index eg: `[0, 6]` denote Sunday and Saturday.

```typescript
openCalendar() {
  const options: CalendarModalOptions = {
    disableWeeks: [0, 6]
  };

  const myCalendar = await this.modalCtrl.create({
    component: CalendarModal,
    componentProps: { options }
  });

  myCalendar.present();

  const event: any = await myCalendar.onDidDismiss();
  const date: CalendarResult = event.data;
  console.log(date);
}
```

### Localization

your root module

```typescript
import { NgModule, LOCALE_ID } from '@angular/core';
...

@NgModule({
  ...
  providers: [{ provide: LOCALE_ID, useValue: "zh-CN" }]
})

...
```

```typescript
openCalendar() {
  const options: CalendarModalOptions = {
    monthFormat: 'YYYY 年 MM 月 ',
    weekdays: ['天', '一', '二', '三', '四', '五', '六'],
    weekStart: 1,
    defaultDate: new Date()
  };

  const myCalendar = await this.modalCtrl.create({
    component: CalendarModal,
    componentProps: { options }
  });

  myCalendar.present();

  const event: any = await myCalendar.onDidDismiss();
  const date: CalendarResult = event.data;
  console.log(date);
}
```

### Days config

Configure one day.

```typescript
openCalendar() {
  let _daysConfig: DayConfig[] = [];
  for (let i = 0; i < 31; i++) {
    _daysConfig.push({
      date: new Date(2017, 0, i + 1),
      subTitle: `$${i + 1}`
    })
  }

  const options: CalendarModalOptions = {
    from: new Date(2017, 0, 1),
    to: new Date(2017, 11.1),
    daysConfig: _daysConfig
  };

  const myCalendar = await this.modalCtrl.create({
    component: CalendarModal,
    componentProps: { options }
  });

  myCalendar.present();

  const event: any = await myCalendar.onDidDismiss();
  const date: CalendarResult = event.data;
  console.log(date);
}
```

# API

### Modal Options

| Name                      | Type                     | Default                               | Description                                                |
| ------------------------- | ------------------------ | ------------------------------------- | ---------------------------------------------------------- |
| from                      | Date                     | `new Date()` | start date                                                 |
| to                        | Date                     | 0 (Infinite)                          | end date                                                   |
| title                     | string                   | `'CALENDAR'` | title                                                      |
| color                     | string                   | `'primary'` | 'primary', 'secondary', 'danger', 'light', 'dark'          |
| defaultScrollTo           | Date                     | none                                  | let the view scroll to the default date                    |
| defaultDate               | Date                     | none                                  | default date data, apply to single                         |
| defaultDates              | Array<Date>              | none                                  | default dates data, apply to multi                         |
| defaultDateRange          | { from: Date, to: Date } | none                                  | default date-range data, apply to range                    |
| defaultTitle              | string                   | ''                                    | default title in days                                      |
| defaultSubtitle           | string                   | ''                                    | default subtitle in days                                   |
| cssClass                  | string                   | `''` | Additional classes for custom styles, separated by spaces. |
| canBackwardsSelected      | boolean                  | `false` | can backwards selected                                     |
| pickMode                  | string                   | `single` | 'multi', 'range', 'single'                                 |
| disableWeeks              | Array<number>            | `[]` | week to be disabled (0-6)                                  |
| closeLabel                | string                   | `CANCEL` | cancel button label                                        |
| doneLabel                 | string                   | `DONE` | done button label                                          |
| clearLabel                | string                   |  null                                 | clear button label                                         |
| closeIcon                 | boolean                  | `false` | show cancel button icon                                    |
| doneIcon                  | boolean                  | `false` | show done button icon                                      |
| monthFormat               | string                   | `'MMM YYYY'` | month title format                                         |
| weekdays                  | Array<string>            | `['S', 'M', 'T', 'W', 'T', 'F', 'S']` | weeks text                                                 |
| weekStart                 | number                   | `0` (0 or 1)                          | set week start day                                         |
| daysConfig                | Array<**_DaysConfig_**>  | `[]` | days configuration                                         |
| step                      | number                   | `12` | month load stepping interval to when scroll                |
| defaultEndDateToStartDate | boolean                  | `false` | makes the end date optional, will default it to the start  |
| maxRange          | number                  | 0 | The maximum range of the selection in days                        |

### onDidDismiss Output `{ data } = event`

| pickMode | Type                                                     |
| -------- | -------------------------------------------------------- |
| single   | { date: **_CalendarResult_** }                           |
| range    | { from: **_CalendarResult_**, to: **_CalendarResult_** } |
| multi    | Array<**_CalendarResult_**>                              |

### onDidDismiss Output `{ role } = event`

| Value      | Description                          |
| ---------- | ------------------------------------ |
| 'cancel'   | dismissed by click the cancel button |
| 'done'     | dismissed by click the done button   |
| 'backdrop' | dismissed by click the backdrop      |

#### DaysConfig

| Name     | Type    | Default  | Description                           |
| -------- | ------- | -------- | ------------------------------------- |
| cssClass | string  | `''` | separated by spaces                   |
| date     | Date    | required | configured days                       |
| marked   | boolean | false    | highlight color                       |
| disable  | boolean | false    | disable                               |
| title    | string  | none     | displayed title eg: `'today'` |
| subTitle | string  | none     | subTitle subTitle eg: `'New Year\'s'` |

### CalendarResult

| Name    | Type   |
| ------- | ------ |
| time    | number |
| unix    | number |
| dateObj | Date   |
| string  | string |
| years   | number |
| months  | number |
| date    | number |

[npm-image]: https://img.shields.io/npm/v/@googlproxer/ion-range-calendar.svg
[npm-url]: https://www.npmjs.com/package/@googlproxer/ion-range-calendar
[license-image]: https://img.shields.io/badge/license-MIT-blue.svg?style=flat
[license-url]: LICENSE
