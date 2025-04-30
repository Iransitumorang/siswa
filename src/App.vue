<template>
  <router-view></router-view>
</template>

<script setup>
// No script needed for now
</script>

<style>
:root {
  --dark-bg: #1a1a1a;
  --dark-text: #ffffff;
  --dark-text-secondary: #e0e0e0;
  --dark-text-muted: #b0b0b0;
  --dark-card: #2d2d2d;
  --dark-border: #404040;
  --dark-input: #333333;
  --dark-hover: #404040;
  --dark-success: #198754;
  --dark-warning: #ffc107;
  --dark-danger: #dc3545;
  --dark-shadow: rgba(0, 0, 0, 0.3);
}

body {
  transition: background-color 0.3s ease, color 0.3s ease;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.dark-mode {
  background-color: var(--dark-bg) !important;
  color: var(--dark-text) !important;
}

.dark-mode h1, 
.dark-mode h2, 
.dark-mode h3, 
.dark-mode h4, 
.dark-mode h5, 
.dark-mode h6 {
  color: var(--dark-text) !important;
  text-shadow: 0 1px 2px var(--dark-shadow) !important;
}

.dark-mode p {
  color: var(--dark-text-secondary) !important;
}

.dark-mode .text-muted {
  color: var(--dark-text-muted) !important;
}

.dark-mode .container {
  background-color: var(--dark-card) !important;
  color: var(--dark-text) !important;
  border: 1px solid var(--dark-border) !important;
  box-shadow: 0 4px 6px var(--dark-shadow) !important;
}

.dark-mode .card {
  background-color: var(--dark-card) !important;
  border-color: var(--dark-border) !important;
  color: var(--dark-text) !important;
  box-shadow: 0 2px 4px var(--dark-shadow) !important;
}

.dark-mode .form-control {
  background-color: var(--dark-input) !important;
  color: var(--dark-text) !important;
  border-color: var(--dark-border) !important;
}

.dark-mode .form-control::placeholder {
  color: var(--dark-text-muted) !important;
}

.dark-mode .form-control:focus {
  background-color: var(--dark-input) !important;
  color: var(--dark-text) !important;
  border-color: var(--dark-success) !important;
  box-shadow: 0 0 0 0.25rem rgba(25, 135, 84, 0.25) !important;
}

.dark-mode .input-group-text {
  background-color: var(--dark-input) !important;
  color: var(--dark-text) !important;
  border-color: var(--dark-border) !important;
}

.dark-mode .table {
  background-color: var(--dark-card) !important;
  color: #000 !important;
}

.dark-mode .table td,
.dark-mode .table th {
  color: #000 !important;
  border-color: var(--dark-border) !important;
}

.dark-mode .table th {
  font-weight: 600 !important;
  letter-spacing: 0.5px !important;
  background-color: #e0e0e0 !important;
}

.dark-mode .table td {
  font-weight: 400 !important;
  background-color: #f5f5f5 !important;
}

.dark-mode .table-striped > tbody > tr:nth-of-type(odd) {
  background-color: #f0f0f0 !important;
}

.dark-mode .table-hover tbody tr:hover {
  background-color: #e8e8e8 !important;
  color: #000 !important;
}

.dark-mode .btn-outline-light:hover {
  background-color: var(--dark-hover) !important;
  color: var(--dark-text) !important;
}

.dark-mode .btn-warning {
  background-color: var(--dark-warning) !important;
  border-color: var(--dark-warning) !important;
  color: #000 !important;
  text-shadow: 0 1px 1px rgba(255, 255, 255, 0.3) !important;
}

.dark-mode .btn-danger {
  background-color: var(--dark-danger) !important;
  border-color: var(--dark-danger) !important;
  color: #fff !important;
  text-shadow: 0 1px 1px rgba(0, 0, 0, 0.3) !important;
}

.dark-mode .btn-success {
  background-color: var(--dark-success) !important;
  border-color: var(--dark-success) !important;
  color: #fff !important;
  text-shadow: 0 1px 1px rgba(0, 0, 0, 0.3) !important;
}

.dark-mode .main-header {
  background: linear-gradient(135deg, #1a1a1a, #2d2d2d) !important;
  color: var(--dark-text) !important;
  text-shadow: 0 2px 4px var(--dark-shadow) !important;
}

.dark-mode .form-section {
  background: linear-gradient(135deg, #1a1a1a, #2d2d2d) !important;
  color: var(--dark-text) !important;
}

.dark-mode .table-header {
  background: linear-gradient(135deg, #1a1a1a, #2d2d2d) !important;
  color: var(--dark-text) !important;
  text-shadow: 0 1px 2px var(--dark-shadow) !important;
}

.dark-mode .main-wrapper {
  background: linear-gradient(135deg, #1a1a1a, #2d2d2d, #1a1a1a) !important;
}

.dark-mode .select2-container--default .select2-selection--single {
  background-color: var(--dark-input) !important;
  border-color: var(--dark-border) !important;
  color: var(--dark-text) !important;
}

.dark-mode .select2-container--default .select2-selection--single .select2-selection__rendered {
  color: var(--dark-text) !important;
}

.dark-mode .select2-dropdown {
  background-color: var(--dark-card) !important;
  border-color: var(--dark-border) !important;
}

.dark-mode .select2-container--default .select2-results__option--highlighted[aria-selected] {
  background-color: var(--dark-success) !important;
  color: white !important;
}

.dark-mode .select2-container--default .select2-results__option {
  color: var(--dark-text) !important;
}

.dark-mode .modal-content {
  background-color: var(--dark-card) !important;
  border-color: var(--dark-border) !important;
  color: var(--dark-text) !important;
  box-shadow: 0 4px 8px var(--dark-shadow) !important;
}

.dark-mode .modal-header {
  border-bottom-color: var(--dark-border) !important;
}

.dark-mode .modal-footer {
  border-top-color: var(--dark-border) !important;
}

.dark-mode .alert {
  background-color: var(--dark-card) !important;
  border-color: var(--dark-border) !important;
  color: var(--dark-text) !important;
  box-shadow: 0 2px 4px var(--dark-shadow) !important;
}

.dark-mode .toast {
  background-color: var(--dark-card) !important;
  color: var(--dark-text) !important;
  box-shadow: 0 2px 4px var(--dark-shadow) !important;
}

.dark-mode .toast-header {
  background-color: var(--dark-input) !important;
  color: var(--dark-text) !important;
  border-bottom-color: var(--dark-border) !important;
}

/* Text-specific improvements */
.dark-mode .navbar-brand {
  color: var(--dark-text) !important;
  text-shadow: 0 1px 2px var(--dark-shadow) !important;
}

.dark-mode .form-label {
  color: var(--dark-text-secondary) !important;
}

.dark-mode .btn {
  font-weight: 500 !important;
  letter-spacing: 0.3px !important;
}

.dark-mode .input-group-text i {
  color: var(--dark-text-secondary) !important;
}

.dark-mode .text-success {
  color: #4caf50 !important;
}

.dark-mode .text-warning {
  color: #ffc107 !important;
}

.dark-mode .text-danger {
  color: #f44336 !important;
}

.dark-mode .text-info {
  color: #2196f3 !important;
}

.dark-mode .text-primary {
  color: #1976d2 !important;
}

.dark-mode .swal2-popup {
  background: var(--dark-card) !important;
  color: var(--dark-text) !important;
  border: 1px solid var(--dark-border) !important;
  box-shadow: 0 4px 16px var(--dark-shadow) !important;
}
.dark-mode .swal2-title,
.dark-mode .swal2-html-container {
  color: var(--dark-text) !important;
}
.dark-mode .swal2-confirm,
.dark-mode .swal2-cancel {
  background-color: var(--dark-success) !important;
  color: #fff !important;
  border: none !important;
}

.dark-mode .btn-light,
.dark-mode button.btn-light {
  background-color: var(--dark-input) !important;
  color: #4caf50 !important;
  border-color: var(--dark-border) !important;
  box-shadow: 0 2px 4px var(--dark-shadow) !important;
}

.dark-mode .btn-light:hover,
.dark-mode button.btn-light:hover {
  background-color: var(--dark-hover) !important;
  color: #43d17a !important;
  border-color: var(--dark-success) !important;
}

.dark-mode, .dark-mode *:not(i) {
  color: #4caf50 !important;
}

.dark-mode input::placeholder,
.dark-mode textarea::placeholder {
  color: #4caf50 !important;
  opacity: 1 !important;
}

.dark-mode button,
.dark-mode .btn,
.dark-mode .btn * {
  color: #4caf50 !important;
}

.dark-mode select,
.dark-mode option {
  color: #4caf50 !important;
}

.dark-mode th,
.dark-mode td,
.dark-mode thead,
.dark-mode tbody,
.dark-mode tfoot,
.dark-mode tr {
  color: #4caf50 !important;
}

.dark-mode table,
.dark-mode table * {
  color: #4caf50 !important;
}
</style>
