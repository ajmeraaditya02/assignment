task-dialog.component.ts

import { Component, Inject, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { MAT_DIALOG_DATA, MatDialogRef } from '@angular/material/dialog';

@Component({
  selector: 'app-task-dialog',
  templateUrl: './task-dialog.component.html',
  styleUrls: ['./task-dialog.component.scss']
})
export class TaskDialogComponent implements OnInit {
  selectedForm: 'assignment' | 'creation' | null = null;
  
  taskAssignmentForm!: FormGroup;
  taskCreationForm!: FormGroup;

  // Dummy dropdown options
  taskTypes = ['Bug Fix', 'Feature', 'Improvement'];
  assignees = ['John Doe', 'Jane Smith', 'Robert Brown'];
  priorityLevels = ['Critical', 'Severe', 'Normal'];
  productTypes = ['Electronics', 'Furniture', 'Clothing'];
  suppliers = ['Supplier A', 'Supplier B', 'Supplier C'];

  constructor(
    private fb: FormBuilder,
    public dialogRef: MatDialogRef<TaskDialogComponent>,
    @Inject(MAT_DIALOG_DATA) public data: any
  ) {}

  ngOnInit() {
    this.createForms();
  }

  createForms() {
    // Form for Task Assignment
    this.taskAssignmentForm = this.fb.group({
      taskType: ['', Validators.required],
      assignee: ['', Validators.required],
      priority: ['', Validators.required],
      date: ['', Validators.required],
      location: ['', Validators.required]
    });

    // Form for Task Creation
    this.taskCreationForm = this.fb.group({
      productType: ['', Validators.required],
      supplier: ['', Validators.required],
      totalQuantity: [null, [Validators.required, Validators.min(1)]],
      price: [null, [Validators.required, Validators.min(0)]],
      date: ['', Validators.required],
      location: ['', Validators.required]
    });
  }

  openForm(type: 'assignment' | 'creation') {
    this.selectedForm = type;
    this.taskAssignmentForm.reset();
    this.taskCreationForm.reset();
  }

  onSubmit() {
    if (this.selectedForm === 'assignment' && this.taskAssignmentForm.valid) {
      console.log('Task Assignment Data:', this.taskAssignmentForm.value);
      this.dialogRef.close(this.taskAssignmentForm.value);
    } else if (this.selectedForm === 'creation' && this.taskCreationForm.valid) {
      console.log('Task Creation Data:', this.taskCreationForm.value);
      this.dialogRef.close(this.taskCreationForm.value);
    }
  }

  closeDialog() {
    this.dialogRef.close();
  }
}


dialog ui

<h2 mat-dialog-title>Task Management</h2>

<!-- Initially Show Buttons -->
<div *ngIf="!selectedForm" class="button-container">
  <button mat-raised-button color="primary" (click)="openForm('assignment')">Task Assignment</button>
  <button mat-raised-button color="accent" (click)="openForm('creation')">Task Creation</button>
</div>

<!-- Task Assignment Form -->
<form *ngIf="selectedForm === 'assignment'" [formGroup]="taskAssignmentForm" (ngSubmit)="onSubmit()">
  <h3>Task Assignment</h3>

  <mat-form-field appearance="outline">
    <mat-label>Task Type</mat-label>
    <mat-select formControlName="taskType">
      <mat-option *ngFor="let type of taskTypes" [value]="type">{{ type }}</mat-option>
    </mat-select>
  </mat-form-field>

  <mat-form-field appearance="outline">
    <mat-label>Assignee</mat-label>
    <mat-select formControlName="assignee">
      <mat-option *ngFor="let assignee of assignees" [value]="assignee">{{ assignee }}</mat-option>
    </mat-select>
  </mat-form-field>

  <mat-form-field appearance="outline">
    <mat-label>Priority Level</mat-label>
    <mat-select formControlName="priority">
      <mat-option *ngFor="let level of priorityLevels" [value]="level">{{ level }}</mat-option>
    </mat-select>
  </mat-form-field>

  <mat-form-field appearance="outline">
    <mat-label>Date</mat-label>
    <input matInput [matDatepicker]="picker1" formControlName="date" />
    <mat-datepicker-toggle matIconSuffix [for]="picker1"></mat-datepicker-toggle>
    <mat-datepicker #picker1></mat-datepicker>
  </mat-form-field>

  <mat-form-field appearance="outline">
    <mat-label>Location</mat-label>
    <input matInput type="text" formControlName="location" />
  </mat-form-field>

  <div class="actions">
    <button mat-button (click)="selectedForm = null">Back</button>
    <button mat-raised-button color="primary" type="submit" [disabled]="taskAssignmentForm.invalid">Submit</button>
  </div>
</form>

<!-- Task Creation Form -->
<form *ngIf="selectedForm === 'creation'" [formGroup]="taskCreationForm" (ngSubmit)="onSubmit()">
  <h3>Task Creation</h3>

  <mat-form-field appearance="outline">
    <mat-label>Product Type</mat-label>
    <mat-select formControlName="productType">
      <mat-option *ngFor="let type of productTypes" [value]="type">{{ type }}</mat-option>
    </mat-select>
  </mat-form-field>

  <mat-form-field appearance="outline">
    <mat-label>Supplier</mat-label>
    <mat-select formControlName="supplier">
      <mat-option *ngFor="let supplier of suppliers" [value]="supplier">{{ supplier }}</mat-option>
    </mat-select>
  </mat-form-field>

  <mat-form-field appearance="outline">
    <mat-label>Total Quantity</mat-label>
    <input matInput type="number" formControlName="totalQuantity" />
  </mat-form-field>

  <mat-form-field appearance="outline">
    <mat-label>Price</mat-label>
    <input matInput type="number" formControlName="price" />
  </mat-form-field>

  <mat-form-field appearance="outline">
    <mat-label>Date</mat-label>
    <input matInput [matDatepicker]="picker2" formControlName="date" />
    <mat-datepicker-toggle matIconSuffix [for]="picker2"></mat-datepicker-toggle>
    <mat-datepicker #picker2></mat-datepicker>
  </mat-form-field>

  <mat-form-field appearance="outline">
    <mat-label>Location</mat-label>
    <input matInput type="text" formControlName="location" />
  </mat-form-field>

  <div class="actions">
    <button mat-button (click)="selectedForm = null">Back</button>
    <button mat-raised-button color="primary" type="submit" [disabled]="taskCreationForm.invalid">Submit</button>
  </div>
</form>


overdue

import { Component } from '@angular/core';
import { MatDialog } from '@angular/material/dialog';
import { TaskDialogComponent } from '../shared/task-dialog/task-dialog.component';

@Component({
  selector: 'app-overdue-charges',
  templateUrl: './overdue-charges.component.html',
  styleUrls: ['./overdue-charges.component.scss']
})
export class OverdueChargesComponent {
  constructor(private dialog: MatDialog) {}

  openDialog() {
    this.dialog.open(TaskDialogComponent, {
      width: '400px'
    });
  }
}


<button mat-raised-button color="primary" (click)="openDialog()">
  Open Task Dialog
</button>
