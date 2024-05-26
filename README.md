ANGULAR
1)	Write a program to configure routing with lazy loading technique?
2)	Write a program to add and retrieve super heroes using services?
3)	Write a program to demonstrate angular life cycle hooks with component communication?
4)	Write a program to send message using angular subject



1 ANS: // app-routing.module.ts
const routes: Routes = [
  {
    path: 'items',
    loadChildren: () => import('./items/items.module').then(m => m.ItemsModule)
  }
];


2 ANS: // user-form.component.ts
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-user-form',
  templateUrl: './user-form.component.html',
  styleUrls: ['./user-form.component.css']
})
export class UserFormComponent implements OnInit {
  userForm: FormGroup;

  constructor(private fb: FormBuilder) {}

  ngOnInit(): void {
    this.userForm = this.fb.group({
      name: ['', Validators.required],
      email: ['', [Validators.required, Validators.email]],
      address: this.fb.group({
        street: ['', Validators.required],
        city: ['', Validators.required],
        zip: ['', Validators.pattern(/^\d{5}$/)]
      })
    });
  }

  onSubmit() {
    if (this.userForm.valid) {
      // Save user data
      console.log(this.userForm.value);
    }
  }
}

// HTML 


<!-- user-form.component.html -->
<form [formGroup]="userForm" (ngSubmit)="onSubmit()">
  <input formControlName="name" placeholder="Name">
  <input formControlName="email" placeholder="Email">
  <div formGroupName="address">
    <input formControlName="street" placeholder="Street">
    <input formControlName="city" placeholder="City">
    <input formControlName="zip" placeholder="ZIP Code">
  </div>
  <button type="submit">Save</button>
</form>



3 ANS: // superhero.service.ts
import { Injectable } from '@angular/core';
import { Superhero } from './superhero.model';

@Injectable({
  providedIn: 'root'
})
export class SuperheroService {
  private superheroes: Superhero[] = [];

  getSuperheroes(): Superhero[] {
    return this.superheroes;
  }

  addSuperhero(superhero: Superhero) {
    this.superheroes.push(superhero);
  }
}


// superhero.model.ts
export class Superhero {
  constructor(public id: number, public name: string) {}
}

4 ANS: // message.service.ts
import { Injectable } from '@angular/core';
import { Subject } from 'rxjs';

@Injectable({ providedIn: 'root' })
export class MessageService {
  private subject = new Subject<string>();

  sendMessage(message: string) {
    this.subject.next(message);
  }

  getMessage() {
    return this.subject.asObservable();
  }
}

// parent.component.ts
import { Component } from '@angular/core';
import { MessageService } from './message.service';

@Component({
  selector: 'app-parent',
  template: `
    <h1>Parent Component</h1>
    <button (click)="sendMessage()">Send Message to Child</button>
    <app-child></app-child>
  `,
})
export class ParentComponent {
  constructor(private messageService: MessageService) {}

  sendMessage() {
    this.messageService.sendMessage('Hello from parent!');
  }
}

// child.component.ts
import { Component, OnDestroy } from '@angular/core';
import { Subscription } from 'rxjs';
import { MessageService } from './message.service';

@Component({
  selector: 'app-child',
  template: `
    <h3>Child Component</h3>
    <p>{{ message }}</p>
  `,
})
export class ChildComponent implements OnDestroy {
  message: string;
  private subscription: Subscription;

  constructor(private messageService: MessageService) {
    this.subscription = this.messageService.getMessage().subscribe((msg) => {
      this.message = msg;
    });
  }

  ngOnDestroy() {
    this.subscription.unsubscribe();
  }
}



5ANS:
// message.service.ts
import { Injectable } from '@angular/core';
import { Subject } from 'rxjs';

@Injectable({ providedIn: 'root' })
export class MessageService {
  private subject = new Subject<string>();

  sendMessage(message: string) {
    this.subject.next(message);
  }

  getMessage() {
    return this.subject.asObservable();
  }
}

// parent.component.ts
import { Component } from '@angular/core';
import { MessageService } from './message.service';

@Component({
  selector: 'app-parent',
  template: `
    <h1>Parent Component</h1>
    <button (click)="sendMessage()">Send Message to Child</button>
    <app-child></app-child>
  `,
})
export class ParentComponent {
  constructor(private messageService: MessageService) {}

  sendMessage() {
    this.messageService.sendMessage('Hello from parent!');
  }
}

// child.component.ts
import { Component, OnDestroy } from '@angular/core';
import { Subscription } from 'rxjs';
import { MessageService } from './message.service';

@Component({
  selector: 'app-child',
  template: `
    <h3>Child Component</h3>
    <p>{{ message }}</p>
  `,
})
export class ChildComponent implements OnDestroy {
  message: string;
  private subscription: Subscription;

  constructor(private messageService: MessageService) {
    this.subscription = this.messageService.getMessage().subscribe((msg) => {
      this.message = msg;
    });
  }

  ngOnDestroy() {
    this.subscription.unsubscribe();
  }
}
