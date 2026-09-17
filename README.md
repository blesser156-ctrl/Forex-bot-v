document.addEventListener("DOMContentLoaded", function () {
    let running = false;
    let connected = false;
    let balance = 10000;
    let pnl = 0;

    const start = document.getElementById("start");
    const stop = document.getElementById("stop");
    const emergency = document.getElementById("emergency");
    const connect = document.getElementById("connect");

    const status = document.getElementById("status");
    const pnlEl = document.getElementById("pnl");
    const balanceEl = document.getElementById("balance");
    const connection = document.getElementById("connection");
    const logEl = document.getElementById("log");

    function log(message) {
        if (logEl) {
            logEl.textContent =
                new Date().toLocaleTimeString() + " — " + message;
        }
    }

    function updateScreen() {
        if (status) {
            status.textContent = running ? "RUNNING" : "STOPPED";
            status.style.color = running ? "#087443" : "#b42318";
        }

        if (balanceEl) {
            balanceEl.textContent = "$" + balance.toFixed(2);
        }

        if (pnlEl) {
            pnlEl.textContent =
                (pnl >= 0 ? "+$" : "-$") +
                Math.abs(pnl).toFixed(2);
        }
    }

    // CONNECT DEMO
    if (connect) {
        connect.addEventListener("click", function () {
            connected = true;

            connection.textContent =
                "MT5: Demo connection ready (simulated)";

            connection.className = "online";

            connect.textContent = "CONNECTED ✓";

            log("Demo connection successful.");
        });
    }

    // START BOT
    if (start) {
        start.addEventListener("click", function () {

            if (!connected) {
                log("Please tap CONNECT DEMO first.");
                connection.textContent =
                    "MT5: Not connected — tap CONNECT DEMO";
                return;
            }

            running = true;

            updateScreen();

            log(
                "Bot started in DEMO mode. No real order will be sent."
            );
        });
    }

    // STOP BOT
    if (stop) {
        stop.addEventListener("click", function () {
            running = false;

            updateScreen();

            log("Bot stopped.");
        });
    }

    // EMERGENCY STOP
    if (emergency) {
        emergency.addEventListener("click", function () {
            running = false;

            updateScreen();

            log(
                "EMERGENCY STOP activated. Automatic trading disabled."
            );
        });
    }

    // Demo market simulation
    setInterval(function () {

        if (!running) {
            return;
        }

        const movement = (Math.random() - 0.5) * 2;

        pnl += movement;
        balance += movement;

        updateScreen();

    }, 3000);

    updateScreen();

    log("Forex Bot V1 loaded. Demo mode ready.");
});
