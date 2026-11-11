<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Period Calendar Widget</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
    }

    body {
      background-color: transparent;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 10px;
    }

    .calendar-card {
      width: 290px;
      background: #ffffff;
      border-radius: 20px;
      padding: 16px 18px;
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
      border: 1px solid #f0f0f0;
      user-select: none;
    }

    /* 顶部 App 风格标题 */
    .app-title {
      text-align: center;
      font-size: 13px;
      font-weight: 600;
      color: #8e8e93;
      letter-spacing: 0.5px;
      margin-bottom: 8px;
      font-style: italic;
    }

    /* 月份导航栏 */
    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 12px;
      padding: 0 4px;
    }

    .month-year {
      font-size: 15px;
      font-weight: 700;
      color: #2c3e50;
    }

    .nav-btn {
      background: none;
      border: none;
      font-size: 14px;
      color: #a0a0a0;
      cursor: pointer;
      padding: 4px 8px;
      border-radius: 6px;
      transition: all 0.2s;
    }

    .nav-btn:hover {
      color: #333;
      background: #f5f5f7;
    }

    /* 星期头部与日历网格 */
    .weekdays, .days {
      display: grid;
      grid-template-columns: repeat(7, 1fr);
      text-align: center;
    }

    .weekdays {
      margin-bottom: 6px;
    }

    .weekday {
      font-size: 11px;
      font-weight: 600;
      color: #b0b0b0;
      padding: 4px 0;
    }

    .days {
      row-gap: 6px;
    }

    .day {
      font-size: 13px;
      font-weight: 500;
      color: #333333;
      height: 32px;
      width: 32px;
      line-height: 32px;
      margin: 0 auto;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      transition: transform 0.15s ease;
    }

    .day:empty {
      cursor: default;
    }

    /* 经期粉红圆圈高亮样式 */
    .day.period {
      background: linear-gradient(135deg, #ff5e7e 0%, #ff3b68 100%);
      color: #ffffff;
      font-weight: 700;
      box-shadow: 0 3px 8px rgba(255, 60, 100, 0.35);
    }

    .day:not(:empty):hover {
      transform: scale(1.08);
    }
  </style>
</head>
<body>

  <div class="calendar-card">
    <!-- 主标题 -->
    <div class="app-title">𝑷𝒆𝒓𝒊𝒐𝒅 𝑪𝒂𝒍𝒆𝒏𝒅𝒂𝒓</div>
    
    <!-- 年月控制栏 -->
    <div class="header">
      <button class="nav-btn" onclick="changeMonth(-1)">&lt;</button>
      <div class="month-year" id="monthYear">2026/11</div>
      <button class="nav-btn" onclick="changeMonth(1)">&gt;</button>
    </div>

    <!-- 英文星期表头 -->
    <div class="weekdays">
      <div class="weekday">Sun</div>
      <div class="weekday">Mon</div>
      <div class="weekday">Tue</div>
      <div class="weekday">Wed</div>
      <div class="weekday">Thu</div>
      <div class="weekday">Fri</div>
      <div class="weekday">Sat</div>
    </div>

    <!-- 日期面板 -->
    <div class="days" id="daysContainer"></div>
  </div>

  <script>
    // 当前显示的年月设置（默认 2026 年 11 月）
    let currentDate = new Date(2026, 10, 1); 

    // 自定义高亮日期 (示例：2026年11月6日 - 12日)
    const periodDates = [
      '2026-11-06', '2026-11-07', '2026-11-08', 
      '2026-11-09', '2026-11-10', '2026-11-11', '2026-11-12'
    ];

    function renderCalendar() {
      const year = currentDate.getFullYear();
      const month = currentDate.getMonth();

      // 更新顶部 2026/11 格式
      const formattedMonth = String(month + 1).padStart(2, '0');
      document.getElementById('monthYear').textContent = `${year}/${formattedMonth}`;

      const firstDayIndex = new Date(year, month, 1).getDay();
      const lastDay = new Date(year, month + 1, 0).getDate();

      const daysContainer = document.getElementById('daysContainer');
      daysContainer.innerHTML = '';

      // 填充当月第一天前的空白格子
      for (let i = 0; i < firstDayIndex; i++) {
        const emptyDiv = document.createElement('div');
        emptyDiv.classList.add('day');
        daysContainer.appendChild(emptyDiv);
      }

      // 渲染日期
      for (let day = 1; day <= lastDay; day++) {
        const dayDiv = document.createElement('div');
        dayDiv.classList.add('day');
        dayDiv.textContent = day;

        const dateStr = `${year}-${formattedMonth}-${String(day).padStart(2, '0')}`;
        if (periodDates.includes(dateStr)) {
          dayDiv.classList.add('period');
        }

        daysContainer.appendChild(dayDiv);
      }
    }

    function changeMonth(delta) {
      currentDate.setMonth(currentDate.getMonth() + delta);
      renderCalendar();
    }

    // 初始化运行
    renderCalendar();
  </script>
</body>
</html>
