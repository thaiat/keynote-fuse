### Create folder resources under app/

### Boostrap components
```ts
import { RainComponent } from './components/rain/rain.component';
import { SnowComponent } from './components/snow/snow.component';
import { SunComponent } from './components/sun/sun.component';
import { MoonComponent } from './components/moon/moon.component';
import { CloudComponent } from './components/cloud/cloud.component';
import { TabComponent } from './components/tab/tab.component';


....

 declarations: [MainComponent, TabComponent, RainComponent, SnowComponent, CloudComponent, SunComponent, MoonComponent],


```

### Create service
```bash
yo mcfly-ng2:service  Weather
```

```ts
import { Injectable } from '@angular/core';
import { Http } from '@angular/http';
import { Observable } from 'rxjs';

const apiUrl = 'http://beta.randomapi.com/api/el3rc6as?key=2D3S-2MX2-EHXY-DDF6';
@Injectable()
export class Weather {
    constructor(private http: Http) {

    }

    get(): Observable<Array<any>> {
        return this.http.get(apiUrl)
            .map(res => {
                return res.json().results[0].times;
            });
    }
}
```

### Modify main component

```ts
import { Component, ChangeDetectionStrategy, ChangeDetectorRef } from '@angular/core';
import { Weather } from '../../services/weather/weather.service.ts';
@Component({
    selector: 'main',
    template: require('./main.component.ngux'),
    providers: [Weather],
    changeDetection: ChangeDetectionStrategy.OnPush
})
export class MainComponent {
    times: Array<any>;

    constructor(private weatherService: Weather, private cd: ChangeDetectorRef) {
        this.reload();
    }

    reload() {
        this.weatherService.get().subscribe(times => {
            this.times = times;
            this.cd.markForCheck();
        });
    }
}
```


ngux
```html
<Panel ng:Selector="main" Background="#eeeeeeff">
    <DockPanel>
        <Font File="../../../../resources/Nunito-Regular.ttf" ux:Global="NunitoRegular" />
        <Font File="../../../../resources/Nunito-Light.ttf" ux:Global="NunitoLight" />
        <Font File="../../../../resources/Nunito-Bold.ttf" ux:Global="NunitoBold" />
        <Text ux:Class="Body" TextColor="#fff" FontSize="18" Alignment="Left" />
        <Fuse.iOS.StatusBarConfig Style="Light" />
        <StackPanel Dock="Top" Background="#8ba892" (Clicked)="reload()">
            <StatusBarBackground/>
            <Text Margin="5,5,0,5" Alignment="Left" FontSize="18" Font="NunitoBold" TextColor="#ffffff80"> Atlanta, Georgia </Text>
        </StackPanel>
        <BottomBarBackground Dock="Bottom" />
        <Panel Dock="Fill" Background="#8ba892">
            <LinearNavigation ux:Name="lnav">
                <NavigationMotion Overflow="Clamp" GotoDuration="0.3" GotoEasing="QuadraticOut" />
            </LinearNavigation>
            <SwipeNavigate SwipeDirection="Down" />

            <Panel *ngFor="let time of times">
                <ng:tab [ng:content]="time" />
            </Panel>

        </Panel>
    </DockPanel>
</Panel>
```

typescript
```ts
import { Component, ChangeDetectionStrategy, ChangeDetectorRef } from '@angular/core';
import { Weather } from '../../services/weather/weather.service.ts';
@Component({
    selector: 'main',
    template: require('./main.component.ngux'),
    providers: [Weather],
    changeDetection: ChangeDetectionStrategy.OnPush
})
export class MainComponent {
    times: Array<any>;

    constructor(private weatherService: Weather, private cd: ChangeDetectorRef) {
        this.reload();
    }

    reload() {
        this.weatherService.get().subscribe(times => {
            this.times = times;
            this.cd.markForCheck();
        });
    }
}

```

### Create tab component
ngux
```html
<Panel ng:Selector="tab">
    <Page ClipToBounds="true" Background="{{content.bgcolor}}">
        <Translation Y="{{content.ypos}}" RelativeTo="Size" />
        <Panel Margin="50,50,0,0">
            <Panel Alignment="TopLeft" Width="30%">
                <Panel>
                    <ng:cloud *ngIf="content.Summary !== 'Sunny'" />
                    <ng:rain *ngIf="content.Summary === 'Rainy'" />
                    <ng:snow *ngIf="content.Summary === 'Snowy'" />
                </Panel>
            </Panel>
            <Panel>
                <ng:tod [ng:name]="content.TOD" Width="25%" Alignment="TopLeft" Offset="-25,-30" />
            </Panel>
            <EnteringAnimation>
                <Move Y="-0.5" RelativeTo="ParentSize" />
            </EnteringAnimation>
            <ExitingAnimation>
                <Move Y="0.1667" RelativeTo="ParentSize" />
            </ExitingAnimation>
        </Panel>
        <StackPanel Width="43%" Alignment="TopRight" Height="100%">
            <Text Value="{{content.TOD}}" Font="NunitoBold" FontSize="19" TextColor="#ffffff80" Margin="0,14,0,0" />
            <Text Value="{{content.Temp}}" Font="NunitoBold" FontSize="32" TextColor="#fff" Margin="0,0,0,14" Alignment="CenterLeft" />
            <StackPanel Alignment="TopLeft">
                <Body Value="{{content.Summary}}" FontSize="26" />
                <Body Value="{{content.Wind}}" />
                <Body Value="{{content.Humidity}}" />
                <ExitingAnimation>
                    <Move Y="0.5" RelativeTo="ParentSize" Easing="Linear" />
                </ExitingAnimation>
                <EnteringAnimation>
                    <Move Y="0.5" RelativeTo="ParentSize" Easing="Linear" />
                </EnteringAnimation>
            </StackPanel>
        </StackPanel>
        <EnteringAnimation>
            <Move Y="0.3334" RelativeTo="Size" />
        </EnteringAnimation>
    </Page>
</Panel>
```

```ts
import { Component, ChangeDetectionStrategy, Input } from '@angular/core';

@Component({
    selector: 'tab',
    template: require('./tab.component.ngux'),
    changeDetection: ChangeDetectionStrategy.OnPush
})
export class TabComponent {
    @Input() content: {};
}

```

## What to do

### Create service
Weather

### Create tod component
* generator
* bootstrap
* ngux 
```html
<Panel ng:Selector="tod">
    <Image *ngIf="name==='Night'" File="../../../../resources/moon.png" />
    <Image *ngIf="name!=='Night'" File="../../../../resources/sun.png" />
</Panel>

```
* typescript
```ts
import { Component, ChangeDetectionStrategy , Input} from '@angular/core';

@Component({
    selector: 'tod',
    template: require('./tod.component.ngux'),
    changeDetection: ChangeDetectionStrategy.OnPush
})
export class TodComponent {
    @Input() name: String;
}
```


