# SCT_TrackCode_Task2
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Stopwatch</title>
    <!-- Bootstrap CSS -->
    <link href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" rel="stylesheet">
    <style>
        /* Body styling */
        body {
            background-color: #000000;
            font-family: Arial, sans-serif;
        }

        /* Stopwatch container styling */
        .stopwatch-container {
            width: 350px;
        }

        /* Time display styling */
        #time-display {
            display: flex;
            justify-content: center;
            font-weight: bold;
            letter-spacing: 3px;
            color: #333;
        }

        /* Digit box styling */
        .time-digit {
            width: 70px;
            height: 70px;
            margin: 0 4px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 1.5rem;
            background-color: #007bff;
            color: white;
            font-family: 'Courier New', Courier, monospace;
            border-radius: 0; /* Square shape */
        }

        /* Lap list styling */
        #laps-list {
            max-height: 200px;
            overflow-y: auto;
        }

        .list-group-item {
            font-size: 1em;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container d-flex justify-content-center align-items-center vh-100">
        <div class="stopwatch-container text-center bg-light p-4 rounded shadow">
            <h1 class="mb-4">Stopwatch</h1>
            <div id="time-display" class="mb-4">
                <span class="time-digit" id="hours">00</span>
                <span class="time-digit">:</span>
                <span class="time-digit" id="minutes">00</span>
                <span class="time-digit">:</span>
                <span class="time-digit" id="seconds">00</span>
                <span class="time-digit">.</span>
                <span class="time-digit" id="milliseconds">00</span>
            </div>
            <div class="buttons mb-3">
                <button id="start-pause" class="btn btn-success mr-2">Start</button>
                <button id="reset" class="btn btn-danger mr-2">Reset</button>
                <button id="lap" class="btn btn-primary">Lap</button>
            </div>
            <ul id="laps-list" class="list-group mt-3"></ul>
        </div>
    </div>

    <!-- Bootstrap and jQuery JS -->
    <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.5.2/dist/js/bootstrap.bundle.min.js"></script>

    <script>
        let startTime = 0;
        let elapsedTime = 0;
        let isRunning = false;
        let interval;
        let lapCounter = 1;

        const hoursEl = document.getElementById('hours');
        const minutesEl = document.getElementById('minutes');
        const secondsEl = document.getElementById('seconds');
        const millisecondsEl = document.getElementById('milliseconds');
        const startPauseBtn = document.getElementById('start-pause');
        const resetBtn = document.getElementById('reset');
        const lapBtn = document.getElementById('lap');
        const lapsList = document.getElementById('laps-list');

        function startPause() {
            if (isRunning) {
                pause();
            } else {
                start();
            }
        }

        function start() {
            isRunning = true;
            startPauseBtn.textContent = 'Pause';
            startPauseBtn.classList.replace('btn-success', 'btn-warning');
            startTime = Date.now() - elapsedTime;
            interval = setInterval(updateTime, 10);
        }

        function pause() {
            isRunning = false;
            startPauseBtn.textContent = 'Start';
            startPauseBtn.classList.replace('btn-warning', 'btn-success');
            elapsedTime = Date.now() - startTime;
            clearInterval(interval);
        }

        function reset() {
            isRunning = false;
            clearInterval(interval);
            startPauseBtn.textContent = 'Start';
            startPauseBtn.classList.replace('btn-warning', 'btn-success');
            elapsedTime = 0;
            startTime = 0;
            updateDisplay(0, 0, 0, 0);
            lapsList.innerHTML = '';
            lapCounter = 1;
        }

        function lap() {
            if (isRunning) {
                const lapTime = formatTime(elapsedTime);
                const lapItem = document.createElement('li');
                lapItem.classList.add('list-group-item');
                lapItem.textContent = `Lap ${lapCounter++}: ${lapTime}`;
                lapsList.appendChild(lapItem);
            }
        }

        function updateTime() {
            elapsedTime = Date.now() - startTime;
            const ms = Math.floor((elapsedTime % 1000) / 10);
            const s = Math.floor((elapsedTime / 1000) % 60);
            const m = Math.floor((elapsedTime / (1000 * 60)) % 60);
            const h = Math.floor((elapsedTime / (1000 * 60 * 60)) % 24);
            updateDisplay(h, m, s, ms);
        }

        function updateDisplay(hours, minutes, seconds, milliseconds) {
            hoursEl.textContent = hours.toString().padStart(2, '0');
            minutesEl.textContent = minutes.toString().padStart(2, '0');
            secondsEl.textContent = seconds.toString().padStart(2, '0');
            millisecondsEl.textContent = milliseconds.toString().padStart(2, '0');
        }

        function formatTime(milliseconds) {
            let ms = milliseconds % 1000;
            let seconds = Math.floor((milliseconds / 1000) % 60);
            let minutes = Math.floor((milliseconds / (1000 * 60)) % 60);
            let hours = Math.floor((milliseconds / (1000 * 60 * 60)) % 24);

            ms = Math.floor(ms / 10);
            seconds = seconds < 10 ? '0' + seconds : seconds;
            minutes = minutes < 10 ? '0' + minutes : minutes;
            hours = hours < 10 ? '0' + hours : hours;

            return `${hours}:${minutes}:${seconds}.${ms}`;
        }

        startPauseBtn.addEventListener('click', startPause);
        resetBtn.addEventListener('click', reset);
        lapBtn.addEventListener('click', lap);
    </script>
</body>
</html>
