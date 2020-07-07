# tour-of-heroes
First angular app from tutorial: https://angular.io/tutorial

## Структура
```bash
# Новый workspace
tour-of-heroes/
  # End-to-end тестовый проект
  e2e/
  src/
    # Первоначальный скелетон проекта
    app/
    # Глобальные стили приложения
    styles.css
```

## Работа
* Создание проекта (выбрал всё по умолчанию), добавляет свой `.gitignore`:
```bash
ng new tour-of-heroes
```
* Запуск проекта (по умолчанию `http://localhost:4200/`):
```bash
cd tour-of-heroes/
ng serve --open # Флаг ставится для открытия в браузере
```
* У проекта горячая перезагрузка - после внесения изменений, они применяются.
* Основные структурные элементы - это компоненты (`components`).
* Компоненты распределены по трём файлам:
  * `*.component.ts` - код компонента, написанный на TypeScript.
  * `*.component.html` - шаблон компонента, написанный на HTML.
  * `*.component.css` - приватные стили данного компонента, написанные на CSS.
* `{{title}}` - будет подгружено значение атрибута `title` из связанного `*.component.ts`.
* Для создания нового компонента использовать команду:
```bash
ng generate component component_name
```
* Эта команда создаёт в папке `app/`:
```bash
heroes/
  heroes.component.css # Создаётся пустым
  heroes.component.html
  heroes.component.spec.ts
  heroes.component.ts
```
* Angular CLI также добавляет компонент в `app.module.ts`, поэтому всё работает.
* Мы всегда импортируем декоратор `@Component` из `@angular/core` для определения метадланных компонента:
  * `selector` - Селектор элементов CSS компонента. В шаблоне теги компонента будут `<app-heroes>`
  * `templateUrl` - путь к шаблону компонента (html).
  * `styleUrls` - путь к файлу с приватными стилями компонента (css).
* `ngOnInit()` - хук жизненнго цикла. Angular вызывает этот метод сразу после создания компонента. Хорошее место для инициализации логики.
* Всегда экспортируйте класс с помощью `export`, чтобы можно было импортировать его с помощью `import` где-нибудь в другом месте.
* Чтобы отобразить компонент (в нашем случае `HeroesComponent`), необходимо добавить его в шаблон `AppComponent`.
* После добавления объекта (это объявление + инициализация):
  ```typescript
  hero: Hero = {
    id: 1,
    name: 'Windstorm'
  };
  ```
  В шаблоне можно использовать так: `{{hero.name}}`.
* Использование Pipe'ов для перенаправления потока выполнения в функцию: `{{hero.name | uppercase }}`.
* Можно использовать как встроенные Pipe'ы, так и создавать свои. Это удобный способ для работы со строками.
* `[(ngModel)]` - это двунаправленный байнд. С помощью него при обновлении значения в `<input>` будет обновляться значение в атрибута в скрипте, а при изменеии азначения атрибута в скрипте будет изменяться значение в `<input>`.
* Angular должен знать, как собрать куски приложения в единое целое. Для этого он использует метаданные.
  * Некоторые метаданные находятся в `@Component`.
  * Особо важные метаданные находятся в `@NgModule`.
  * Самый важный декоратор `@NgModule` создаёт аннотации для `AppModule`.
* Каждый компонент должен быть объявлен только в одном `NgModule`.
* Все компоненты должны добавляться в `AppModule`.
  * Компоненты добалвяются в блок `declarations`.
  * Модули добавляются в блок `imports`.
* Директива `*ngFor` повторит всё, в т.ч. и хостовой элемент (`<li>`):
  ```html
  <li *ngFor="let hero of heroes">
  ```
* Приватный стили инлайнятся в `@Component.styles` массиве или определяются как ccs-файлы в `@Component.styleUrls` массиве.
* Привязка события к методу:
  ```html
  <li *ngFor="let hero of heroes" (click)="onSelect(hero)">
  ```
* Директива `*ngIf` проверяет условие:
  ```html
  <div *ngIf="selectedHero">
  ```
* Директива `[class.selected]="hero === selectedHero"` позволяет применять класс по условию.
  ```html
  <li *ngFor="let hero of heroes" [class.selected]="hero === selectedHero"></li>
  ```
* Директива `@Input()`:
  * Похоже, что это аналог проброса пропсов во Vue.js. И мы объявляем этот декоратор, чтобы показать, что значение будет проброшено из родительского элемента.
  * Шаблон `HeroDetailComponent` связан с атрибутом `hero`.
  * Атрибут `Hero` должен быть input-атрибутом, задекорированным с помощью `@input()`, потому что внешний `HeroesComponent` будет байндить к нему так.
    ```html
    <app-hero-detail [hero]="selectedHero"></app-hero-detail>
    ```
  * Для этого нужно добавить: `import { Input } from '@angular/core';`
* Родительский компонент (`HeroesComponent`) будет контролировать дочерний компонент (`HeroDetailComponent`) с помощью отправки ему нового герия для отображения, когда пользователь выбирает геория из списка.
* Property binding: `[hero]="selectedHero"`.
  * Это однонаправленный байндинг из атрибута `selectedHero` (`HeroesComponent`) в атрибут `hero` целевого элемента, который маппит `hero` у `HeroDetailComponent`.
  * Теперь, когда пользователь кликает по герою в списке, `selectedHero` изменяется. Когда `selectedHero` изменяется, property binding обновляет `hero` и `HeroDetailComponent` отображает нового героя.
* Создание сервиса:
  ```bash
  ng generate service hero
  ```
* Эта команда создаёт в папке `app/`:
  ```bash
  hero.service.ts
  hero.service.spec.ts
  ```
* Декоратор `@Injectable` - используется для класса, который использоуется в Depencey Injection системе.
* Декоратор `@Injectable` принимает объект метаданных для сервиса, аналогично `@Component`.
* Provider - что-то, что может создавать или доставлять сервисы; в нашем случае, он инстанцирует класс `HeroService` для предоставления сервиса.
* Для того, чтобы удостовериться в том, что `heroService` может предоставить этот сервис, зарегиструем его с помощью injector, который является объектом, который отвечает за выбор и внедрение provider'а, где приложение его требует.
* По умолчанию команда `ng generate service` регистрирует provider с root injector для серсиса за счёт включения метаданных provider, которыми является `providedIn: 'root'` в декораторе `@Injectable`.
  ```angular2
  @Injectable({
    providedIn: 'root'
  })
  ```
* Когда вы предоставляете сервис на root-уровне, Angular создаёт один расшаренный инстанс `HeroService` и внедряет его в любой класс, который потребует этого. Регистрация provider в `@Injectable` метаданных также позволяет Algular оптимизировать приложения за счёт удаления сервиса, если выяснится, что он не используется.
* Приватный параметр:
  ```angular2
  constructor(private heroService: HeroService) {}
  ```
  * Этот параметр одновременно объявляет приватный атрибут `heroSevice` и идентифицирует его как `HeroService`.
  * Когда Angular создаёт `HeroesComponent`, DI система устанавливает `heroService` параметр в синглтон-инстанс `HeroService`.
* Конструктор используется для простой инициализации: запись параметров в атрибуты. Конструктор НЕ ДОЛЖЕН ничего делать.
* Вместо этого нужно запихнуть `getHeroes()` в хук `ngOnInit()` и позволить Angular вызвать `ngOnInit()` в подходящее время, после того, как инстанс `HeroesComponent` был создан.
* `Observable` - ключевой класс в библиотеке `RxJS`.
* В `hero.service.ts` переписал так:
  ```angular2
  import { Observable, of } from 'rxjs';
  
  getHeroes(): Observable<Hero[]> {
    return of(HEROES);
  }
  ```
  * `of(HEROES)` - возвращает `Observable<Hero[]>`, который выдаёт одно значение, массив замоканных георев.
* В `heroes.component.ts` переписал так:
  ```angular2
  getHeroes(): void {
    this.heroService.getHeroes().subscribe(heroes => this.heroes = heroes);
  }
  ```
  * Это асинхронный подход, в отличие от предыдущего варианта.
  * `getHeroes()` возвращает `Observable`, на который мы подписываемся с помощью `.subscribe()`.
  * Текущая версия ждёт, когда `Observable` выдаст список георев - это может произойти сейчас или несколькими минутами позже.
  * Метод `subscribe()` передаёт выданный массив в callback, который устанавливает его в атрибут `heroes`.
* Чтобы компонент отображался, его нужно добавить в `app.component.html`: `<app-messages></app-messages>`.
* Если компонент вставился успешно, то мы увидим дефолтную надпись `<component_name> works!`.
* Типичный "service-to-service" сценарий: `MessageService` инжектится в `HeroService`, который в свою очередь инжектися в `heroesComponent`.
* Атрибуты должны быть `public`, если собираемся их байндить в шаблоне.
* В `message.service.ts` добавляли:
  ```angular2
  constructor(public messageService: MessageService) {}
  ```
  В `messages.component.html` можно вызывать атрибуты сервиса:
  ```html
  <div *ngIf="messageService.messages.length">
    <h2>Messages</h2>
    <button class="clear" (click)="messageService.clear()">clear</button>
    <div *ngFor='let message of messageService.messages'> {{message}} </div>
  </div>
  ```
* Подведение итогов части 4:
  * Мы зарегистрировали `HeroService` как провайдера его сервиса на root уровне, чтобы его можно было инжектить где-угодно в приложении.
  * Мы использовали Angular Depencency Injection для инжектинга его в компонент.
  * Мы дали `HeroService` асинхронный метод.
  * Мы использовали `of()` для возвращения `Observable` списка героев (`Observable<Hero[]>`).
  * Хук `ngOnInit` используется для вызова метода `HeroService`, а не конструктор.
  * Мы создали `MessageService` для нетесносвязанного взаимодействия между классами.
  * `HeroSerivce`, заинжектеный в комопонент, создаётся с помощью другого заинжектенного сервиса, `MessageService`.