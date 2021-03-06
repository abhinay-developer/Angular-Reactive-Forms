# Angular-Reactive-Forms
Reactive forms
app.component.ts
================

import { Component, OnInit, VERSION } from "@angular/core";
import { FormArray, FormBuilder, FormGroup, Validators } from "@angular/forms";

@Component({
  selector: "my-app",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent implements OnInit {
 
  userDetails: FormGroup;

  constructor(private fb: FormBuilder) {}

  public userDetailsFromGroupOnInit() {
    this.userDetails = this.fb.group({
      firstName: ["", [Validators.required]],
      lastName: ["", [Validators.required]],
      mobileNos: this.fb.array([
        this.fb.group({
          mobileNo: ["", [Validators.required]]
        })
      ])
    });
  }

  public get MobileDetails(): FormArray {
    return this.userDetails.get("mobileNos") as FormArray;
  }

  ngOnInit() {
    this.userDetailsFromGroupOnInit();
  }
  onAdd() {
    const mobile = this.fb.group({
      mobileNo: ["", [Validators.required]]
    });
    this.MobileDetails.push(mobile);
  }

  onRemove(i) {
    this.MobileDetails.removeAt(i);
  }
  onRegister() {
    console.log(this.userDetails.value);
  }
}

app.component.html
===================

<form [formGroup]="userDetails">
  <label>First Name</label>
  <input type="text"  formControlName="firstName" id="firstName" name="firstName"><br><br>


  <label>Last Name</label>
  <input type="text"  formControlName="lastName" id="lastName" name="lastName"><br><br><br>


  <label>Mobile No</label> &nbsp;&nbsp;&nbsp;
  <button (click)="onAdd()">Add</button><br><br>
  <div *ngFor="let item of userDetails.controls.mobileNos['controls'];let i=index" formArrayName="mobileNos">
    <div [formGroupName]="i">
      <input type="number"  formControlName="mobileNo" id="mobileNo" name="mobileNo">
      <button (click)="onRemove(i)">Remove</button>
    </div>
  </div>
  <br>

  <div>
    <button (click)="onRegister()">Register</button><br><br>
  </div>
</form>


app.module.ts
================

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule, ReactiveFormsModule } from '@angular/forms';

import { AppComponent } from './app.component';
import { HelloComponent } from './hello.component';

@NgModule({
  imports:      [ BrowserModule, FormsModule ,ReactiveFormsModule],
  declarations: [ AppComponent],
  bootstrap:    [ AppComponent]
})
export class AppModule { }
