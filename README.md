<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Crypto Alarms Widget</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      padding: 20px;
      min-height: 100vh;
      --theme-color: #00C805;
    }

    input[type="number"]::-webkit-inner-spin-button,
    input[type="number"]::-webkit-outer-spin-button {
      -webkit-appearance: none;
      margin: 0;
    }
    input[type="number"] {
      -moz-appearance: textfield;
    }

    input[type="radio"] {
      appearance: none;
      -webkit-appearance: none;
      width: 14px;
      height: 14px;
      border: 1px solid #666;
      border-radius: 3px;
      background-color: #1a1a1a;
      cursor: pointer;
    }
    input[type="radio"]:checked {
      background-color: var(--theme-color);
      border-color: var(--theme-color);
    }

    /* Round ball radio buttons for time unit */
    input[type="radio"][name="timeUnit"] {
      width: 10px;
      height: 10px;
      border-radius: 2px;
      border: 1px solid #666;
    }

    input[type="radio"][name="timeUnit"]:checked {
      background-color: var(--theme-color);
      border-color: var(--theme-color);
    }

    .main-panel {
      position: fixed;
      background-color: rgba(0, 0, 0, 0.7);
      backdrop-filter: blur(10px);
      -webkit-backdrop-filter: blur(10px);
      width: 400px;
      height: 400px;
      color: #fff;
      padding: 10px;
      overflow: hidden;
      border-radius: 8px;
      box-shadow: 0 4px 20px rgba(0,0,0,0.5);
      left: 50px;
      top: 50px;
      border: 1px solid rgba(255, 255, 255, 0.1);
      min-width: 300px;
      min-height: 300px;
      max-width: 800px;
      max-height: 800px;
    }

    .resize-handle {
      position: absolute;
      width: 10px;
      height: 10px;
      background-color: transparent;
      border-radius: 2px;
      z-index: 10;
    }

    .resize-handle:hover {
      background-color: rgba(255, 255, 255, 0.1);
    }

    .resize-nw {
      top: 0;
      left: 0;
      cursor: nw-resize;
    }

    .resize-ne {
      top: 0;
      right: 0;
      cursor: ne-resize;
    }

    .resize-sw {
      bottom: 0;
      left: 0;
      cursor: sw-resize;
    }

    .resize-se {
      bottom: 0;
      right: 0;
      cursor: se-resize;
    }

    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 8px;
      cursor: grab;
      user-select: none;
    }

    .header.dragging {
      cursor: grabbing;
    }

    .green-square {
      width: 14px;
      height: 14px;
      background-color: #00C805;
      border-radius: 2px;
      cursor: pointer;
      transition: opacity 0.2s;
    }

    .green-square:hover {
      opacity: 0.8;
    }

    .header-buttons {
      display: flex;
      gap: 8px;
    }

    .header-buttons button {
      background: none;
      border: none;
      color: #fff;
      font-size: 16px;
      cursor: pointer;
      padding: 0;
      line-height: 1;
    }

    h1 {
      font-size: 16px;
      font-weight: 400;
      margin-bottom: 10px;
      letter-spacing: -0.3px;
    }

    table {
      width: 100%;
      border-collapse: collapse;
    }

    thead tr {
      border-bottom: 1px solid #2c2c2c;
    }

    th {
      padding: 6px 4px;
      text-align: right;
      font-size: 8px;
      font-weight: 500;
      color: #b3b3b3;
      letter-spacing: 0.1px;
      white-space: nowrap;
    }

    th:nth-child(2) {
      text-align: left;
      cursor: pointer;
    }

    th:nth-child(2):hover,
    th:nth-child(3):hover,
    th:nth-child(4):hover,
    th:nth-child(5):hover {
      color: #fff;
    }

    th:nth-child(3),
    th:nth-child(4),
    th:nth-child(5) {
      cursor: pointer;
    }

    td {
      padding: 6px 4px;
      text-align: right;
      font-size: 9px;
      color: #fff;
      font-weight: 400;
      white-space: nowrap;
      border-bottom: 1px solid #1a1a1a;
    }

    td:nth-child(1) {
      text-align: center;
    }

    td:nth-child(2) {
      text-align: left;
      font-weight: 500;
    }

    /* Custom Scrollbar */
    .table-container {
      overflow-y: auto;
      overflow-x: hidden;
      max-height: calc(100% - 100px);
    }

    .table-container::-webkit-scrollbar {
      width: 6px;
    }

    .table-container::-webkit-scrollbar-track {
      background: transparent;
    }

    .table-container::-webkit-scrollbar-thumb {
      background-color: rgba(0, 0, 0, 0.7);
      border-radius: 3px;
      backdrop-filter: blur(10px);
      -webkit-backdrop-filter: blur(10px);
    }

    .table-container::-webkit-scrollbar-thumb:hover {
      background-color: rgba(0, 0, 0, 0.9);
    }

    /* For Firefox */
    .table-container {
      scrollbar-width: thin;
      scrollbar-color: rgba(0, 0, 0, 0.7) transparent;
    }

    .crypto-symbol {
      display: flex;
      align-items: center;
      gap: 4px;
    }

    .symbol-icon {
      font-size: 8px;
      opacity: 0.6;
    }

    .change-negative {
      color: #FF5000;
    }

    .arrow-icon {
      font-size: 8px;
    }

    .trash-btn {
      background: none;
      border: none;
      cursor: pointer;
      padding: 0;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .trash-icon {
      width: 11px;
      height: 11px;
      color: #ff8888;
    }

    .trash-btn:hover .trash-icon {
      color: #ff5555;
    }

    .delete-all-btn {
      position: absolute;
      bottom: 15px;
      right: 15px;
      background: none;
      border: none;
      color: #ff6b6b;
      font-size: 9px;
      cursor: pointer;
      opacity: 0.7;
      padding: 4px 8px;
      transition: opacity 0.2s;
      display: none;
    }

    .delete-all-btn.visible {
      display: block;
    }

    .delete-all-btn:hover {
      opacity: 1;
    }

    .add-panel {
      position: fixed;
      background-color: rgba(0, 0, 0, 0.7);
      backdrop-filter: blur(10px);
      -webkit-backdrop-filter: blur(10px);
      width: 180px;
      color: #fff;
      padding: 10px;
      border-radius: 8px;
      box-shadow: 0 4px 20px rgba(0,0,0,0.5);
      border: 1px solid rgba(255, 255, 255, 0.1);
      left: 470px;
      top: 50px;
      display: none;
    }

    .add-panel.visible {
      display: block;
    }

    /* Color picker panel */
    .color-picker-panel {
      position: fixed;
      background-color: rgba(0, 0, 0, 0.7);
      backdrop-filter: blur(10px);
      -webkit-backdrop-filter: blur(10px);
      width: 160px;
      color: #fff;
      padding: 8px;
      border-radius: 6px;
      box-shadow: 0 4px 20px rgba(0,0,0,0.5);
      border: 1px solid rgba(255, 255, 255, 0.1);
      left: 50px;
      top: 80px;
      display: none;
    }

    .color-picker-panel.visible {
      display: block;
    }

    .color-grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 6px;
    }

    .color-option {
      width: 30px;
      height: 30px;
      border-radius: 3px;
      cursor: pointer;
      transition: transform 0.2s, box-shadow 0.2s;
      border: 1.5px solid transparent;
    }

    .color-option:hover {
      transform: scale(1.1);
      box-shadow: 0 4px 12px rgba(255, 255, 255, 0.3);
    }

    .color-option.selected {
      border: 1.5px solid #fff;
      box-shadow: 0 0 8px rgba(255, 255, 255, 0.4);
    }

    .color-green {
      background-color: #00C805;
    }

    .color-orange {
      background-color: #FF8C00;
    }

    .color-red {
      background-color: #FF5000;
    }

    .color-purple {
      background-color: #764ba2;
    }

    .color-blue {
      background-color: #1E90FF;
    }

    .color-yellow {
      background-color: #FFD700;
    }

    .color-black {
      background-color: #000000;
    }

    .color-white {
      background-color: #FFFFFF;
    }

    .form-group {
      margin-bottom: 6px;
    }

    .form-label {
      display: block;
      font-size: 10px;
      color: #b3b3b3;
      margin-bottom: 3px;
    }

    .form-input {
      width: 100%;
      background-color: #1a1a1a;
      border: 1px solid #333;
      border-radius: 4px;
      padding: 6px 8px;
      color: #fff;
      font-size: 11px;
      outline: none;
    }

    .price-input-wrapper {
      position: relative;
    }

    .dollar-sign {
      position: absolute;
      left: 8px;
      top: 50%;
      transform: translateY(-50%);
      color: #b3b3b3;
      font-size: 11px;
      z-index: 1;
    }

    .price-input {
      padding-left: 20px;
      padding-right: 30px;
    }

    .spinner-buttons {
      position: absolute;
      right: 4px;
      top: 50%;
      transform: translateY(-50%);
      display: flex;
      flex-direction: column;
      gap: 1px;
    }

    .spinner-btn {
      background: transparent;
      border: none;
      color: #999;
      font-size: 8px;
      cursor: pointer;
      padding: 2px 4px;
      line-height: 1;
    }

    .trigger-group {
      display: flex;
      gap: 8px;
      align-items: center;
      justify-content: space-between;
    }

    .radio-group {
      display: flex;
      gap: 8px;
    }

    .radio-label {
      display: flex;
      align-items: center;
      gap: 4px;
      cursor: pointer;
      font-size: 11px;
      color: #fff;
    }

    .add-btn {
      background-color: var(--theme-color);
      border: none;
      border-radius: 4px;
      padding: 4px 10px;
      color: #fff;
      font-size: 9px;
      font-weight: 400;
      cursor: pointer;
    }

    .td-trash {
      text-align: center;
      padding-left: 20px;
    }

    select.form-input {
      cursor: pointer;
    }

    /* Preferences menu */
    .preferences-panel {
      position: fixed;
      background-color: rgba(0, 0, 0, 0.7);
      backdrop-filter: blur(10px);
      -webkit-backdrop-filter: blur(10px);
      width: 120px;
      color: #fff;
      padding: 10px;
      border-radius: 8px;
      box-shadow: 0 4px 20px rgba(0,0,0,0.5);
      border: 1px solid rgba(255, 255, 255, 0.1);
      right: calc(100vw - 450px - 120px);
      top: 80px;
      display: none;
    }

    .preferences-panel.visible {
      display: block;
    }

    .preferences-item {
      padding: 8px 10px;
      font-size: 11px;
      cursor: pointer;
      border-radius: 4px;
      transition: background-color 0.2s;
      margin-bottom: 4px;
    }

    .preferences-item:hover {
      background-color: rgba(255, 255, 255, 0.1);
    }

    .preferences-item.close {
      color: #ff5555;
    }

    .opacity-control {
      padding: 6px 10px;
      margin-bottom: 6px;
    }

    .opacity-label {
      font-size: 10px;
      color: #b3b3b3;
      margin-bottom: 6px;
      display: block;
    }

    .opacity-slider {
      width: 100%;
      height: 4px;
      background: #333;
      border-radius: 2px;
      outline: none;
      -webkit-appearance: none;
      cursor: pointer;
    }

    .opacity-slider::-webkit-slider-thumb {
      -webkit-appearance: none;
      appearance: none;
      width: 12px;
      height: 12px;
      background: var(--theme-color);
      border-radius: 50%;
      cursor: pointer;
    }

    .opacity-slider::-moz-range-thumb {
      width: 12px;
      height: 12px;
      background: var(--theme-color);
      border-radius: 50%;
      cursor: pointer;
      border: none;
    }

    /* Cookie Checkbox */
    .cookie-checkbox-wrapper {
      display: flex;
      align-items: center;
      cursor: pointer;
      position: relative;
    }

    .cookie-checkbox {
      appearance: none;
      -webkit-appearance: none;
      width: 24px;
      height: 11px;
      background-color: #1a1a1a;
      border: 1px solid #666;
      border-radius: 2px;
      cursor: pointer;
      position: relative;
      transition: all 0.3s;
    }

    .cookie-checkbox:hover {
      border-color: #999;
    }

    .cookie-checkbox::before {
      content: '';
      position: absolute;
      width: 8px;
      height: 8px;
      background-color: #666;
      left: 1px;
      top: 50%;
      transform: translateY(-50%);
      transition: all 0.3s;
      border-radius: 1px;
    }

    .cookie-checkbox:checked {
      background-color: var(--theme-color);
      border-color: var(--theme-color);
    }

    .cookie-checkbox:checked::before {
      background-color: #fff;
      left: 13px;
    }

    /* Custom Sound Dropdown */
    .custom-select-wrapper {
      position: relative;
      width: 120px;
    }

    .condition-select-wrapper {
      width: 100%;
    }

    .condition-icon {
      width: 13px;
      height: 13px;
      color: #999;
      stroke: #999;
      margin-right: 6px;
      flex-shrink: 0;
    }

    .custom-option {
      display: flex;
      align-items: center;
      gap: 0;
    }

    #selectedCondition {
      display: flex;
      align-items: center;
    }

    .custom-select-trigger {
      width: 100%;
      background-color: rgba(26, 26, 26, 0.6);
      border: 1px solid rgba(255, 255, 255, 0.1);
      border-radius: 4px;
      padding: 6px 8px;
      color: #fff;
      font-size: 10px;
      cursor: pointer;
      display: flex;
      justify-content: space-between;
      align-items: center;
      transition: all 0.2s;
    }

    .custom-select-trigger:hover {
      border-color: rgba(255, 255, 255, 0.2);
      background-color: rgba(26, 26, 26, 0.8);
    }

    .custom-select-options {
      position: absolute;
      top: 100%;
      left: 0;
      right: 0;
      background-color: rgba(26, 26, 26, 0.95);
      backdrop-filter: blur(20px);
      -webkit-backdrop-filter: blur(20px);
      border: 1px solid rgba(255, 255, 255, 0.1);
      border-radius: 6px;
      margin-top: 4px;
      display: none;
      z-index: 100;
      max-height: 150px;
      overflow-y: auto;
      box-shadow: 0 8px 24px rgba(0, 0, 0, 0.3);
      padding: 4px;
    }

    .custom-select-options.active {
      display: block;
      animation: slideDown 0.2s ease-out;
    }

    /* Custom scrollbar for dropdown */
    .custom-select-options::-webkit-scrollbar {
      width: 4px;
    }

    .custom-select-options::-webkit-scrollbar-track {
      background: transparent;
    }

    .custom-select-options::-webkit-scrollbar-thumb {
      background-color: #333;
      border-radius: 2px;
    }

    .custom-select-options::-webkit-scrollbar-thumb:hover {
      background-color: #444;
    }

    /* For Firefox */
    .custom-select-options {
      scrollbar-width: thin;
      scrollbar-color: #333 transparent;
    }

    @keyframes slideDown {
      from {
        opacity: 0;
        transform: translateY(-10px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    .custom-option {
      padding: 6px 8px;
      color: #fff;
      font-size: 10px;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: space-between;
      transition: all 0.2s;
      border-radius: 4px;
      margin-bottom: 1px;
    }

    .custom-option:hover {
      background-color: rgba(255, 255, 255, 0.08);
      transform: translateX(2px);
    }

    .custom-option.selected {
      background-color: rgba(255, 255, 255, 0.12);
    }

    .play-icon {
      color: #999;
      font-size: 10px;
      display: flex;
      align-items: center;
      cursor: pointer;
      padding: 2px;
      border-radius: 2px;
      transition: background-color 0.2s;
    }

    .play-icon:hover {
      background-color: rgba(255, 255, 255, 0.1);
    }

    .play-icon svg {
      width: 12px;
      height: 12px;
      stroke: #999;
      pointer-events: none;
    }

    .dropdown-arrow {
      font-size: 8px;
      color: #999;
    }
  </style>
</head>
<body>

  <!-- Main Panel -->
  <div class="main-panel" id="mainPanel">
    <div class="resize-handle resize-nw" data-direction="nw"></div>
    <div class="resize-handle resize-ne" data-direction="ne"></div>
    <div class="resize-handle resize-sw" data-direction="sw"></div>
    <div class="resize-handle resize-se" data-direction="se"></div>
    
    <div class="header" id="header">
      <div class="green-square" id="colorSquare"></div>
      <div class="header-buttons">
        <button id="toggleAddPanel">+</button>
        <button id="togglePreferencesPanel">⋮</button>
      </div>
    </div>

    <h1>Alarms</h1>

    <div class="table-container" id="tableContainer">
      <table>
        <thead>
          <tr>
            <th></th>
            <th id="symbolHeader">Symbol ↑</th>
            <th id="currentPriceHeader">Current price</th>
            <th>Change %</th>
            <th id="targetPriceHeader">Target price</th>
            <th></th>
          </tr>
        </thead>
        <tbody id="cryptoTableBody">
          <!-- Rows will be added dynamically -->
        </tbody>
      </table>
    </div>

    <button class="delete-all-btn" id="deleteAllBtn">Delete all</button>
  </div>

  <!-- Color Picker Panel -->
  <div class="color-picker-panel" id="colorPickerPanel">
    <div class="color-grid">
      <div class="color-option color-green selected" data-color="#00C805"></div>
      <div class="color-option color-orange" data-color="#FF8C00"></div>
      <div class="color-option color-red" data-color="#FF5000"></div>
      <div class="color-option color-purple" data-color="#764ba2"></div>
      <div class="color-option color-blue" data-color="#1E90FF"></div>
      <div class="color-option color-yellow" data-color="#FFD700"></div>
      <div class="color-option color-black" data-color="#000000"></div>
      <div class="color-option color-white" data-color="#FFFFFF"></div>
    </div>
  </div>

  <!-- Preferences Panel -->
  <div class="preferences-panel" id="preferencesPanel">
    <div class="opacity-control">
      <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 6px;">
        <label class="opacity-label" style="margin-bottom: 0;">Opacity</label>
        <span class="opacity-label" style="margin-bottom: 0;" id="opacityPercentage">70%</span>
      </div>
      <input type="range" min="0.3" max="1" step="0.1" value="0.7" class="opacity-slider" id="opacitySlider">
    </div>
    <div class="opacity-control">
      <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 6px;">
        <label class="opacity-label" style="margin-bottom: 0;">Volume</label>
        <span class="opacity-label" style="margin-bottom: 0;" id="volumePercentage">70%</span>
      </div>
      <input type="range" min="0" max="100" step="10" value="70" class="opacity-slider" id="volumeSlider">
    </div>
    <div class="opacity-control">
      <label class="opacity-label" style="margin-bottom: 6px;">Save alarms in cookies</label>
      <label class="cookie-checkbox-wrapper">
        <input type="checkbox" id="saveCookiesCheckbox" class="cookie-checkbox">
        <span class="checkbox-custom"></span>
      </label>
    </div>
    <div class="opacity-control">
      <label class="opacity-label" style="margin-bottom: 6px; line-height: 1.3;">Refresh tab when<br>disconnected</label>
      <label class="cookie-checkbox-wrapper">
        <input type="checkbox" id="refreshOnDisconnectCheckbox" class="cookie-checkbox">
        <span class="checkbox-custom"></span>
      </label>
    </div>
    <div class="preferences-item close" id="closeBtn">Close</div>
  </div>

  <!-- Add Panel -->
  <div class="add-panel" id="addPanel">
    <div class="form-group">
      <label class="form-label">Symbol</label>
      <input type="text" class="form-input" id="symbolInput" placeholder="e.g. OPEN">
    </div>

    <div class="form-group">
      <label class="form-label">Condition</label>
      <div class="custom-select-wrapper condition-select-wrapper" id="conditionSelectWrapper">
        <div class="custom-select-trigger" id="conditionSelectTrigger">
          <span id="selectedCondition">
            <svg class="condition-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M2 20l4-8 4 4 6-10 6 4"/>
            </svg>
            Above the price
          </span>
          <span class="dropdown-arrow">▼</span>
        </div>
        <div class="custom-select-options" id="conditionSelectOptions">
          <div class="custom-option selected" data-value="above">
            <svg class="condition-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M2 20l4-8 4 4 6-10 6 4"/>
            </svg>
            <span>Above the price</span>
          </div>
          <div class="custom-option" data-value="below">
            <svg class="condition-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M22 4l-4 8-4-4-6 10-6-4"/>
            </svg>
            <span>Below the price</span>
          </div>
          <div class="custom-option" data-value="equals">
            <svg class="condition-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 12h14M5 16h14"/>
            </svg>
            <span>Equals</span>
          </div>
        </div>
      </div>
    </div>

    <div class="form-group">
      <label class="form-label">Target price</label>
      <div class="price-input-wrapper">
        <span class="dollar-sign">$</span>
        <input type="number" class="form-input price-input" id="priceInput" placeholder="0.00" step="0.01">
        <div class="spinner-buttons">
          <button class="spinner-btn" id="increaseBtn">▲</button>
          <button class="spinner-btn" id="decreaseBtn">▼</button>
        </div>
      </div>
    </div>

    <div class="form-group">
      <label class="form-label">Sound</label>
      <div class="custom-select-wrapper" id="customSelectWrapper">
        <div class="custom-select-trigger" id="customSelectTrigger">
          <span id="selectedSound">1</span>
          <span class="dropdown-arrow">▼</span>
        </div>
        <div class="custom-select-options" id="customSelectOptions">
          <div class="custom-option selected" data-value="1">
            <span>1</span>
            <span class="play-icon">
              <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" fill="none" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z"/>
              </svg>
            </span>
          </div>
          <div class="custom-option" data-value="2">
            <span>2</span>
            <span class="play-icon">
              <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" fill="none" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z"/>
              </svg>
            </span>
          </div>
          <div class="custom-option" data-value="3">
            <span>3</span>
            <span class="play-icon">
              <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" fill="none" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z"/>
              </svg>
            </span>
          </div>
          <div class="custom-option" data-value="4">
            <span>4</span>
            <span class="play-icon">
              <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" fill="none" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z"/>
              </svg>
            </span>
          </div>
          <div class="custom-option" data-value="5">
            <span>5</span>
            <span class="play-icon">
              <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" fill="none" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z"/>
              </svg>
            </span>
          </div>
        </div>
      </div>
    </div>

    <div class="form-group" id="timeGroup">
      <label class="form-label">Time</label>
      <div style="display: flex; gap: 6px; align-items: center;">
        <div class="price-input-wrapper" style="width: 38px;">
          <input type="number" class="form-input" id="timeInput" placeholder="5" style="width: 100%; text-align: center; padding-right: 0;" min="1" max="99">
          <div class="spinner-buttons">
            <button class="spinner-btn" id="timeIncreaseBtn">▲</button>
            <button class="spinner-btn" id="timeDecreaseBtn">▼</button>
          </div>
        </div>
        <div style="display: flex; gap: 8px; align-items: center;">
          <label class="radio-label" style="font-size: 9px;">
            <input type="radio" name="timeUnit" value="sec" id="timeUnitSec" checked>
            sec
          </label>
          <label class="radio-label" style="font-size: 9px;">
            <input type="radio" name="timeUnit" value="min" id="timeUnitMin">
            min
          </label>
        </div>
      </div>
    </div>

    <div class="form-group">
      <label class="form-label">Trigger</label>
      <div class="trigger-group">
        <div class="radio-group">
          <label class="radio-label">
            <input type="radio" name="trigger" value="Once" id="triggerOnce" checked>
            Once
          </label>
          <label class="radio-label">
            <input type="radio" name="trigger" value="Always">
            Always
          </label>
        </div>
        <button class="add-btn" id="addBtn">Add</button>
      </div>
    </div>
  </div>

  <script>
    // Data
    let cryptos = [];
    let currentColor = '#00C805';

    // Elements
    const mainPanel = document.getElementById('mainPanel');
    const header = document.getElementById('header');
    const addPanel = document.getElementById('addPanel');
    const colorPickerPanel = document.getElementById('colorPickerPanel');
    const preferencesPanel = document.getElementById('preferencesPanel');
    const colorSquare = document.getElementById('colorSquare');
    const toggleAddPanelBtn = document.getElementById('toggleAddPanel');
    const togglePreferencesBtn = document.getElementById('togglePreferencesPanel');
    const closeBtn = document.getElementById('closeBtn');
    const symbolInput = document.getElementById('symbolInput');
    const conditionSelectTrigger = document.getElementById('conditionSelectTrigger');
    const conditionSelectOptions = document.getElementById('conditionSelectOptions');
    const selectedCondition = document.getElementById('selectedCondition');
    let currentSelectedCondition = 'above';
    const priceInput = document.getElementById('priceInput');
    const increaseBtn = document.getElementById('increaseBtn');
    const decreaseBtn = document.getElementById('decreaseBtn');
    const addBtn = document.getElementById('addBtn');
    const deleteAllBtn = document.getElementById('deleteAllBtn');
    const cryptoTableBody = document.getElementById('cryptoTableBody');
    const triggerOnce = document.getElementById('triggerOnce');
    const timeGroup = document.getElementById('timeGroup');
    const timeInput = document.getElementById('timeInput');
    const opacitySlider = document.getElementById('opacitySlider');
    const opacityPercentage = document.getElementById('opacityPercentage');
    const volumeSlider = document.getElementById('volumeSlider');
    const volumePercentage = document.getElementById('volumePercentage');
    const tableContainer = document.getElementById('tableContainer');
    const customSelectTrigger = document.getElementById('customSelectTrigger');
    const customSelectOptions = document.getElementById('customSelectOptions');
    const selectedSound = document.getElementById('selectedSound');
    let currentSelectedSound = '1';
    let currentVolume = 70;
    let sortDescending = true; // Start with high to low for target price
    let symbolSortAscending = true; // For symbol A-Z / Z-A toggle
    let priceSortDescending = true; // For current price high to low

    // Resize functionality
    let isResizing = false;
    let resizeDirection = '';
    let startX, startY, startWidth, startHeight, startLeft, startTop;

    document.querySelectorAll('.resize-handle').forEach(handle => {
      handle.addEventListener('mousedown', (e) => {
        isResizing = true;
        resizeDirection = e.target.dataset.direction;
        startX = e.clientX;
        startY = e.clientY;
        
        const rect = mainPanel.getBoundingClientRect();
        startWidth = rect.width;
        startHeight = rect.height;
        startLeft = rect.left;
        startTop = rect.top;
        
        e.stopPropagation();
      });
    });

    document.addEventListener('mousemove', (e) => {
      if (isResizing) {
        const deltaX = e.clientX - startX;
        const deltaY = e.clientY - startY;
        
        let newWidth = startWidth;
        let newHeight = startHeight;
        let newLeft = startLeft;
        let newTop = startTop;
        
        if (resizeDirection.includes('e')) {
          newWidth = startWidth + deltaX;
        }
        if (resizeDirection.includes('w')) {
          newWidth = startWidth - deltaX;
          newLeft = startLeft + deltaX;
        }
        if (resizeDirection.includes('s')) {
          newHeight = startHeight + deltaY;
        }
        if (resizeDirection.includes('n')) {
          newHeight = startHeight - deltaY;
          newTop = startTop + deltaY;
        }
        
        // Apply constraints
        newWidth = Math.max(300, Math.min(800, newWidth));
        newHeight = Math.max(300, Math.min(800, newHeight));
        
        mainPanel.style.width = newWidth + 'px';
        mainPanel.style.height = newHeight + 'px';
        
        if (resizeDirection.includes('w') || resizeDirection.includes('n')) {
          mainPanel.style.left = newLeft + 'px';
          mainPanel.style.top = newTop + 'px';
        }
        
        // Scale content based on size
        const scale = Math.min(newWidth / 400, newHeight / 400);
        const fontSize = Math.max(0.7, Math.min(1.3, scale));
        mainPanel.style.fontSize = fontSize + 'em';
        
        // Update other panels positions
        const mainRect = mainPanel.getBoundingClientRect();
        addPanel.style.left = (mainRect.right + 20) + 'px';
        addPanel.style.top = mainRect.top + 'px';
        colorPickerPanel.style.left = mainRect.left + 'px';
        colorPickerPanel.style.top = (mainRect.top + 30) + 'px';
        preferencesPanel.style.left = (mainRect.right - 120) + 'px';
        preferencesPanel.style.top = (mainRect.top + 30) + 'px';
      }
    });

    document.addEventListener('mouseup', () => {
      isResizing = false;
    });

    // Keyboard shortcuts
    document.addEventListener('keydown', (e) => {
      // Press "+" to open Add Panel
      if (e.key === '+' || (e.key === '=' && e.shiftKey)) {
        e.preventDefault();
        addPanel.classList.toggle('visible');
        colorPickerPanel.classList.remove('visible');
        preferencesPanel.classList.remove('visible');
        console.log('Add panel toggled');
        return;
      }
      
      // Press Cmd/Ctrl + Number (1-9) to delete that item
      if ((e.metaKey || e.ctrlKey) && e.key >= '1' && e.key <= '9') {
        e.preventDefault();
        const index = parseInt(e.key) - 1;
        console.log(`Cmd/Ctrl + ${e.key} pressed`);
        console.log(`Attempting to delete item at index ${index}`);
        console.log(`Total items in list: ${cryptos.length}`);
        
        if (index >= 0 && index < cryptos.length) {
          const itemToDelete = cryptos[index];
          console.log(`Deleting item:`, itemToDelete);
          deleteCrypto(itemToDelete.id);
          console.log(`✓ Deleted item ${parseInt(e.key)}`);
        } else {
          console.log(`✗ Index ${index} out of range`);
        }
        return;
      }
    });

    // Symbol header click to sort alphabetically
    document.getElementById('symbolHeader').addEventListener('click', () => {
      cryptos.sort((a, b) => {
        if (symbolSortAscending) {
          return a.symbol.localeCompare(b.symbol); // A to Z
        } else {
          return b.symbol.localeCompare(a.symbol); // Z to A
        }
      });
      symbolSortAscending = !symbolSortAscending; // Toggle for next click
      renderTable();
    });

    // Current price header click to sort by price
    document.getElementById('currentPriceHeader').addEventListener('click', () => {
      cryptos.sort((a, b) => {
        if (priceSortDescending) {
          return b.currentPrice - a.currentPrice; // High to low
        } else {
          return a.currentPrice - b.currentPrice; // Low to high
        }
      });
      priceSortDescending = !priceSortDescending; // Toggle for next click
      renderTable();
    });

    // Target price header click to sort
    document.getElementById('targetPriceHeader').addEventListener('click', () => {
      cryptos.sort((a, b) => {
        if (sortDescending) {
          return b.targetPrice - a.targetPrice; // High to low
        } else {
          return a.targetPrice - b.targetPrice; // Low to high
        }
      });
      sortDescending = !sortDescending; // Toggle for next click
      renderTable();
    });

    // Custom select dropdown functionality (Sound)
    customSelectTrigger.addEventListener('click', (e) => {
      e.stopPropagation();
      customSelectOptions.classList.toggle('active');
      conditionSelectOptions.classList.remove('active');
    });

    // Condition dropdown functionality
    conditionSelectTrigger.addEventListener('click', (e) => {
      e.stopPropagation();
      conditionSelectOptions.classList.toggle('active');
      customSelectOptions.classList.remove('active');
    });

    // Handle condition option selection
    document.querySelectorAll('#conditionSelectOptions .custom-option').forEach(option => {
      option.addEventListener('click', (e) => {
        e.stopPropagation();
        
        // Remove selected class from all options
        document.querySelectorAll('#conditionSelectOptions .custom-option').forEach(opt => {
          opt.classList.remove('selected');
        });
        
        // Add selected class to clicked option
        option.classList.add('selected');
        
        // Update selected value and icon
        currentSelectedCondition = option.dataset.value;
        selectedCondition.innerHTML = option.innerHTML;
        
        // Close dropdown
        conditionSelectOptions.classList.remove('active');
      });
    });

    // Handle option selection
    document.querySelectorAll('.custom-option').forEach(option => {
      option.addEventListener('click', (e) => {
        e.stopPropagation();
        
        // Check if play icon was clicked
        if (e.target.closest('.play-icon')) {
          const soundValue = option.dataset.value;
          console.log('Playing sound:', soundValue);
          // Add your sound playing logic here
          return; // Don't select the option, just play the sound
        }
        
        // Remove selected class from all options
        document.querySelectorAll('.custom-option').forEach(opt => {
          opt.classList.remove('selected');
        });
        
        // Add selected class to clicked option
        option.classList.add('selected');
        
        // Update selected value
        currentSelectedSound = option.dataset.value;
        selectedSound.textContent = currentSelectedSound;
        
        // Close dropdown
        customSelectOptions.classList.remove('active');
      });
    });

    // Close dropdown when clicking outside
    document.addEventListener('click', (e) => {
      if (!document.getElementById('customSelectWrapper').contains(e.target)) {
        customSelectOptions.classList.remove('active');
      }
      if (!document.getElementById('conditionSelectWrapper').contains(e.target)) {
        conditionSelectOptions.classList.remove('active');
      }
    });

    // Limit time input to 2 digits
    timeInput.addEventListener('input', (e) => {
      if (e.target.value.length > 2) {
        e.target.value = e.target.value.slice(0, 2);
      }
      if (parseInt(e.target.value) > 99) {
        e.target.value = 99;
      }
    });

    // Opacity slider functionality
    opacitySlider.addEventListener('input', (e) => {
      const opacityValue = e.target.value;
      const opacityPercent = Math.round(opacityValue * 100);
      opacityPercentage.textContent = opacityPercent + '%';
      mainPanel.style.backgroundColor = `rgba(0, 0, 0, ${opacityValue})`;
      addPanel.style.backgroundColor = `rgba(0, 0, 0, ${opacityValue})`;
      colorPickerPanel.style.backgroundColor = `rgba(0, 0, 0, ${opacityValue})`;
      preferencesPanel.style.backgroundColor = `rgba(0, 0, 0, ${opacityValue})`;
      
      // Update scrollbar opacity
      updateScrollbarOpacity(opacityValue);
    });

    // Function to update scrollbar opacity dynamically
    function updateScrollbarOpacity(opacity) {
      const styleId = 'dynamic-scrollbar-style';
      let styleElement = document.getElementById(styleId);
      
      if (!styleElement) {
        styleElement = document.createElement('style');
        styleElement.id = styleId;
        document.head.appendChild(styleElement);
      }
      
      styleElement.textContent = `
        .table-container::-webkit-scrollbar-thumb {
          background-color: rgba(0, 0, 0, ${opacity}) !important;
        }
        .table-container {
          scrollbar-color: rgba(0, 0, 0, ${opacity}) transparent !important;
        }
      `;
    }

    // Volume slider functionality
    volumeSlider.addEventListener('input', (e) => {
      currentVolume = e.target.value;
      volumePercentage.textContent = currentVolume + '%';
      console.log('Volume set to:', currentVolume + '%');
      // Here you can add actual volume control for sounds
    });

    // Cookie checkbox functionality
    const saveCookiesCheckbox = document.getElementById('saveCookiesCheckbox');
    const refreshOnDisconnectCheckbox = document.getElementById('refreshOnDisconnectCheckbox');
    
    // Load saved state from cookies
    function loadFromCookies() {
      const savedCryptos = getCookie('cryptoAlarms');
      if (savedCryptos) {
        try {
          cryptos = JSON.parse(savedCryptos);
          renderTable();
        } catch (e) {
          console.error('Error loading from cookies:', e);
        }
      }
      
      const savedCheckboxState = getCookie('saveCookiesEnabled');
      if (savedCheckboxState === 'true') {
        saveCookiesCheckbox.checked = true;
      }
      
      const savedRefreshState = getCookie('refreshOnDisconnect');
      if (savedRefreshState === 'true') {
        refreshOnDisconnectCheckbox.checked = true;
      }
    }
    
    // Save to cookies
    function saveToCookies() {
      if (saveCookiesCheckbox.checked) {
        setCookie('cryptoAlarms', JSON.stringify(cryptos), 365);
        setCookie('saveCookiesEnabled', 'true', 365);
      }
    }
    
    // Cookie helper functions
    function setCookie(name, value, days) {
      const expires = new Date();
      expires.setTime(expires.getTime() + days * 24 * 60 * 60 * 1000);
      document.cookie = `${name}=${value};expires=${expires.toUTCString()};path=/`;
    }
    
    function getCookie(name) {
      const nameEQ = name + '=';
      const ca = document.cookie.split(';');
      for (let i = 0; i < ca.length; i++) {
        let c = ca[i];
        while (c.charAt(0) === ' ') c = c.substring(1, c.length);
        if (c.indexOf(nameEQ) === 0) return c.substring(nameEQ.length, c.length);
      }
      return null;
    }
    
    // Save cookies checkbox change event
    saveCookiesCheckbox.addEventListener('change', () => {
      if (saveCookiesCheckbox.checked) {
        saveToCookies();
      } else {
        // Clear cookies
        document.cookie = 'cryptoAlarms=;expires=Thu, 01 Jan 1970 00:00:00 UTC;path=/';
        document.cookie = 'saveCookiesEnabled=;expires=Thu, 01 Jan 1970 00:00:00 UTC;path=/';
      }
    });
    
    // Refresh on disconnect checkbox change event
    refreshOnDisconnectCheckbox.addEventListener('change', () => {
      if (refreshOnDisconnectCheckbox.checked) {
        setCookie('refreshOnDisconnect', 'true', 365);
        console.log('Auto-refresh on disconnect: ENABLED');
      } else {
        document.cookie = 'refreshOnDisconnect=;expires=Thu, 01 Jan 1970 00:00:00 UTC;path=/';
        console.log('Auto-refresh on disconnect: DISABLED');
      }
    });
    
    // Monitor connection status
    window.addEventListener('offline', () => {
      console.log('Connection lost...');
      if (refreshOnDisconnectCheckbox.checked) {
        console.log('Will refresh when connection is restored');
      }
    });
    
    window.addEventListener('online', () => {
      console.log('Connection restored!');
      if (refreshOnDisconnectCheckbox.checked) {
        console.log('Refreshing page...');
        setTimeout(() => {
          location.reload();
        }, 1000);
      }
    });
    
    // Load from cookies on page load
    loadFromCookies();

    // Toggle color picker panel
    colorSquare.addEventListener('click', (e) => {
      e.stopPropagation();
      colorPickerPanel.classList.toggle('visible');
      addPanel.classList.remove('visible');
      preferencesPanel.classList.remove('visible');
    });

    // Toggle preferences panel
    togglePreferencesBtn.addEventListener('click', (e) => {
      e.stopPropagation();
      preferencesPanel.classList.toggle('visible');
      addPanel.classList.remove('visible');
      colorPickerPanel.classList.remove('visible');
    });

    // Close button functionality
    closeBtn.addEventListener('click', () => {
      preferencesPanel.classList.remove('visible');
    });

    // Color selection
    document.querySelectorAll('.color-option').forEach(option => {
      option.addEventListener('click', (e) => {
        // Remove selected class from all options
        document.querySelectorAll('.color-option').forEach(opt => {
          opt.classList.remove('selected');
        });
        
        // Add selected class to clicked option
        e.target.classList.add('selected');
        
        // Update color square
        currentColor = e.target.dataset.color;
        colorSquare.style.backgroundColor = currentColor;
        
        // Update theme color for all elements
        document.body.style.setProperty('--theme-color', currentColor);
        
        // Hide color picker panel
        colorPickerPanel.classList.remove('visible');
      });
    });

    // Close panels when clicking outside
    document.addEventListener('click', (e) => {
      if (!colorPickerPanel.contains(e.target) && !colorSquare.contains(e.target)) {
        colorPickerPanel.classList.remove('visible');
      }
      if (!addPanel.contains(e.target) && !toggleAddPanelBtn.contains(e.target)) {
        addPanel.classList.remove('visible');
      }
      if (!preferencesPanel.contains(e.target) && !togglePreferencesBtn.contains(e.target)) {
        preferencesPanel.classList.remove('visible');
      }
    });

    // Toggle time group when Once is selected - REMOVED, keeping time always visible

    // Drag functionality
    let isDragging = false;
    let dragOffset = { x: 0, y: 0 };

    header.addEventListener('mousedown', (e) => {
      // Don't start dragging if clicking on resize handles
      if (e.target.classList.contains('resize-handle')) return;
      // Don't start dragging if clicking on the color square
      if (e.target === colorSquare) return;
      
      isDragging = true;
      header.classList.add('dragging');
      const rect = mainPanel.getBoundingClientRect();
      dragOffset.x = e.clientX - rect.left;
      dragOffset.y = e.clientY - rect.top;
    });

    document.addEventListener('mousemove', (e) => {
      if (isDragging) {
        mainPanel.style.left = (e.clientX - dragOffset.x) + 'px';
        mainPanel.style.top = (e.clientY - dragOffset.y) + 'px';
        
        // Move panels too
        const mainRect = mainPanel.getBoundingClientRect();
        addPanel.style.left = (mainRect.right + 20) + 'px';
        addPanel.style.top = mainRect.top + 'px';
        colorPickerPanel.style.left = mainRect.left + 'px';
        colorPickerPanel.style.top = (mainRect.top + 30) + 'px';
        preferencesPanel.style.left = (mainRect.right - 120) + 'px';
        preferencesPanel.style.top = (mainRect.top + 30) + 'px';
      }
    });

    document.addEventListener('mouseup', () => {
      isDragging = false;
      header.classList.remove('dragging');
    });

    // Toggle add panel
    toggleAddPanelBtn.addEventListener('click', () => {
      addPanel.classList.toggle('visible');
      colorPickerPanel.classList.remove('visible');
      preferencesPanel.classList.remove('visible');
    });

    // Increase/Decrease price
    increaseBtn.addEventListener('click', () => {
      const currentValue = parseFloat(priceInput.value) || 0;
      priceInput.value = (currentValue + 0.01).toFixed(2);
    });

    decreaseBtn.addEventListener('click', () => {
      const currentValue = parseFloat(priceInput.value) || 0;
      priceInput.value = Math.max(0, currentValue - 0.01).toFixed(2);
    });

    // Increase/Decrease time
    const timeIncreaseBtn = document.getElementById('timeIncreaseBtn');
    const timeDecreaseBtn = document.getElementById('timeDecreaseBtn');

    timeIncreaseBtn.addEventListener('click', () => {
      const currentValue = parseInt(timeInput.value) || 0;
      timeInput.value = Math.min(99, currentValue + 1);
    });

    timeDecreaseBtn.addEventListener('click', () => {
      const currentValue = parseInt(timeInput.value) || 0;
      timeInput.value = Math.max(1, currentValue - 1);
    });

    // Format currency
    function formatCurrency(num) {
      if (num >= 1000) {
        return `$${num.toLocaleString('en-US', { minimumFractionDigits: 2, maximumFractionDigits: 2 })}`;
      }
      return `$${num.toFixed(5)}`;
    }

    // Render table
    function renderTable() {
      cryptoTableBody.innerHTML = '';
      
      // Show or hide delete all button based on list content
      if (cryptos.length > 0) {
        deleteAllBtn.classList.add('visible');
      } else {
        deleteAllBtn.classList.remove('visible');
      }
      
      cryptos.forEach((crypto, index) => {
        const row = document.createElement('tr');
        row.innerHTML = `
          <td>${index + 1}</td>
          <td>
            <div class="crypto-symbol">
              ${crypto.symbol}
              ${crypto.symbol === 'BTC' ? '<span class="symbol-icon">₿</span>' : ''}
              ${crypto.symbol === 'DOGE' ? '<span class="symbol-icon" style="font-size: 7px;">Ð ₿</span>' : ''}
            </div>
          </td>
          <td>${formatCurrency(crypto.currentPrice)}</td>
          <td class="change-negative">
            <span class="arrow-icon">▼</span> ${Math.abs(crypto.changePercent)}%
          </td>
          <td>${formatCurrency(crypto.targetPrice)}</td>
          <td class="td-trash">
            <button class="trash-btn" onclick="deleteCrypto(${crypto.id})">
              <svg class="trash-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
              </svg>
            </button>
          </td>
        `;
        cryptoTableBody.appendChild(row);
      });
      
      // Save to cookies if enabled
      saveToCookies();
    }

    // Delete crypto
    function deleteCrypto(id) {
      cryptos = cryptos.filter(crypto => crypto.id !== id);
      renderTable();
    }

    // Delete all
    deleteAllBtn.addEventListener('click', () => {
      cryptos = [];
      renderTable();
    });

    // Add new crypto
    addBtn.addEventListener('click', () => {
      const symbol = symbolInput.value.toUpperCase().trim();
      const condition = currentSelectedCondition;
      const targetPrice = parseFloat(priceInput.value);
      const trigger = document.querySelector('input[name="trigger"]:checked').value;

      if (symbol && targetPrice) {
        const newCrypto = {
          id: Date.now(),
          symbol: symbol,
          condition: condition,
          currentPrice: 0,
          changePercent: 0,
          targetPrice: targetPrice,
          trigger: trigger
        };
        
        cryptos.push(newCrypto);
        renderTable();
        
        // Reset form
        symbolInput.value = '';
        currentSelectedCondition = 'above';
        selectedCondition.innerHTML = `
          <svg class="condition-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M2 20l4-8 4 4 6-10 6 4"/>
          </svg>
          Above the price
        `;
        document.querySelectorAll('#conditionSelectOptions .custom-option').forEach(opt => {
          opt.classList.remove('selected');
        });
        document.querySelector('#conditionSelectOptions .custom-option[data-value="above"]').classList.add('selected');
        priceInput.value = '';
        document.querySelector('input[name="trigger"][value="Once"]').checked = true;
        timeInput.value = '';
        addPanel.classList.remove('visible');
      }
    });

    // Auto-uppercase symbol input
    symbolInput.addEventListener('input', (e) => {
      e.target.value = e.target.value.toUpperCase();
    });

    // Initial render
    renderTable();
  </script>

</body>
</html>
