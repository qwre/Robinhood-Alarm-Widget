<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Robinhood Alarm Widget – Preview</title>
  <style>
    :root {
      --bg-top: #6fa9ff;
      --bg-mid: #7868ff;
      --bg-bottom: #8b46ff;
      --card-bg1: #080c26;
      --card-bg2: #05071a;
      --text-main: #ffffff;
      --text-sub: #8f96c7;
      --accent: #19ff3b;
      --divider: #363a63;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: system-ui, -apple-system, BlinkMacSystemFont,
        "Segoe UI", sans-serif;
    }

    body {
      min-height: 100vh;
      background: linear-gradient(
        180deg,
        var(--bg-top) 0%,
        var(--bg-mid) 45%,
        var(--bg-bottom) 100%
      );
      padding: 70px 0 0 110px; /* paneli sol üstte konumlandırma */
    }

    .panel {
      width: 520px;
      height: 360px;
      border-radius: 18px;
      background: linear-gradient(135deg, var(--card-bg1), var(--card-bg2));
      box-shadow:
        0 26px 70px rgba(0, 0, 0, 0.55),
        0 0 0 1px rgba(255, 255, 255, 0.03);
      color: var(--text-main);
      padding: 18px 24px;
      display: flex;
      flex-direction: column;
      gap: 16px;
    }

    .panel-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
    }

    .panel-header-left {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .status-dot {
      width: 14px;
      height: 14px;
      border-radius: 4px;
      background: var(--accent);
      box-shadow: 0 0 10px rgba(25, 255, 59, 0.7);
    }

    .panel-title {
      font-size: 19px;
      font-weight: 500;
    }

    .panel-header-right {
      display: flex;
      gap: 10px;
      font-size: 18px;
      color: #c2c7ff;
    }

    .panel-header-right span {
      padding: 4px;
      border-radius: 8px;
      cursor: default;
    }

    .panel-header-right span:hover {
      background: rgba(255, 255, 255, 0.06);
    }

    .table-header {
      display: grid;
      grid-template-columns: 2fr 1.2fr 1.2fr 1.2fr;
      font-size: 12px;
      color: var(--text-sub);
      padding: 6px 0 8px 0;
      border-bottom: 1px solid var(--divider);
    }

    .table-header span {
      text-align: left;
    }

    .table-body {
      flex: 1;
      display: flex;
      align-items: center;
      justify-content: center;
      color: var(--text-sub);
      font-size: 13px;
      opacity: 0.45;
    }
  </style>
</head>
<body>

  <div class="panel">
    <div class="panel-header">
      <div class="panel-header-left">
        <div class="status-dot"></div>
        <div class="panel-title">Alarms</div>
      </div>
      <div class="panel-header-right">
        <span>+</span>
        <span>⋮</span>
      </div>
    </div>

    <div class="table-header">
      <span>Symbol</span>
      <span>Current price</span>
      <span>Change %</span>
      <span>Target price</span>
    </div>

    <div class="table-body">
      No alarms yet.
    </div>
  </div>

</body>
</html>
