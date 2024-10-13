const readline = require('readline');
const os = require('os');
const { exec } = require('child_process'); // Import exec for running commands

// ASCII Art
console.log("\x1b[31m%s\x1b[0m", 
    "(                        )\n" +
    "|\\    _,--------._    / |\n" +
    "| `.,'            `. /  |\n" +
    "`  '              ,-'   '\n" +
    " \\/_         _   (     /\n" +
    "(,-.`.    ,',-.`. `_,/\n" +
    " |/#\\ ),-','#\\`= ,'.` |\n" +
    " `._/)  -'.\\_,'   ) ))|\n" +
    " /  (_.)\\     .   -'//\n" +
    "(  /\\____/\\    ) )`'\\\n" +
    " \\ |V----V||  ' ,    \\\n" +
    "  |`- -- -'   ,'   \\  \\      _____\n" +
    "___    |         .'    \\ \\  `._,-'     `-.\n" +
    "   `.__,`---^---'       \\ ` -'\n" +
    "      -.                /  /\n\n" +
    "SSSSS   AAAAA  TTTTTT   AAAAA  N     N  AAAAA  \n" +
    "S       A     A   TT    A     A NN    N A     A \n" +
    " SSSSS  AAAAAAA   TT    AAAAAAA N N   N AAAAAAA \n" +
    "      S A     A   TT    A     A N  N  N A     A \n" +
    "SSSSS   A     A   TT    A     A N   N N A     A "
);

// Function to simulate downloading
function simulateDownload(message, callback) {
    const total = 30; // Total length of the progress bar
    let progress = 0; // Initial progress

    const interval = setInterval(() => {
        progress++;

        // Create a visual representation of the progress
        const percentage = (progress / total) * 100;
        const filledLength = Math.floor(total * (progress / total));
        const bar = "█".repeat(filledLength) + "-".repeat(total - filledLength);

        // Clear the line and redraw the progress bar
        process.stdout.write(`\r\x1b[31m[${bar}] ${percentage.toFixed(2)}%\x1b[0m`);

        // Stop the simulation when the download is complete
        if (progress >= total) {
            clearInterval(interval);
            console.log(`\n\x1b[32m${message}\x1b[0m`);
            callback();
        }
    }, 100); // Update every 100 milliseconds
}

// Sequentially simulate four downloads
function startSimulation() {
    simulateDownload("✅ system32 just got deleted", () => {
        simulateDownload("✅ app data got deleted", () => {
            simulateDownload("⚠️ Windows is going to lock down after 3 days", askUser);
        });
    });
}

// Ask user if they are scared
function askUser() {
    const userName = os.userInfo().username; // Get the username of the PC
    const rl = readline.createInterface({
        input: process.stdin,
        output: process.stdout
    });

    rl.question(`\x1b[31m${userName}, are you scared? (yes/no):\x1b[0m `, (answer) => {
        if (answer.toLowerCase() === 'yes') {
            console.log("\x1b[32mOk buddy, don't be scared.\x1b[0m");
            setTimeout(() => {
                rl.close();
                process.exit(); // Close the app
            }, 500); // Close after 0.5 seconds
        } else if (answer.toLowerCase() === 'no') {
            console.log("\x1b[31mGoodbye little boy!\x1b[0m");
            rl.close();
            playNukeSirenAndShutdown(); // Play nuke siren and start countdown
        } else {
            console.log("\x1b[31mInvalid response. Try again.\x1b[0m");
            askUser(); // Repeat the question if invalid answer
        }
    });
}

// Function to play nuke siren and display countdown
function playNukeSirenAndShutdown() {
    const nukeFilePath = 'C:\\Users\\soldier\\Downloads\\discord bot\\nuke1.mp3'; // Ensure path is correct

    // Play the nuke siren sound using ffplay
    exec(`ffplay -nodisp -autoexit "${nukeFilePath}"`, (error, stdout, stderr) => {
        if (error) {
            console.error(`Could not play sound: ${error.message}`);
            return;
        }
        if (stderr) {
            console.error(`ffplay stderr: ${stderr}`);
            return;
        }
        console.log(`Playing sound...`);
    });

    // 90-second countdown timer
    let countdown = 36; // Set countdown time
    const countdownInterval = setInterval(() => {
        process.stdout.write(`\r\x1b[31mShutting down in ${countdown} seconds...\x1b[0m`);
        countdown--;

        if (countdown < 0) {
            clearInterval(countdownInterval);
            console.log("\n\x1b[31mShutting down now...\x1b[0m"); 
            exec('shutdown /s /f /t 0'); // Shutdown the system forcefully
        }
    }, 1000); // Update every second
}

// Start the download simulation
startSimulation();
//that is the source code 
