#include <QApplication>
#include <QWidget>
#include <QPushButton>
#include <QTextEdit>
#include <QVBoxLayout>
#include <QPalette>
#include <stack>
#include <queue>
#include <iostream>

using namespace std;


stack<int> undoStack;       // Stack for undoing actions
queue<int> playerQueue;     // Queue for players waiting for their turn


void pushAction(int action) {
    undoStack.push(action);
}

void popAction() {
    if (!undoStack.empty()) {
        undoStack.pop();
    }
}

int peekStack() {
    return (!undoStack.empty()) ? undoStack.top() : -1; 
}


void enqueuePlayer(int playerId) {
    playerQueue.push(playerId);
}

void dequeuePlayer() {
    if (!playerQueue.empty()) {
        playerQueue.pop();
    }
}

int peekQueue() {
    return (!playerQueue.empty()) ? playerQueue.front() : -1; 
}

// -----------------------------------------Display current stack and queue states--------------------------------------
void displayStates(QTextEdit* displayBox) {
    stack<int> tempStack = undoStack;
    queue<int> tempQueue = playerQueue;

    QString displayText;
    displayText += "Current Stack (Undo Actions): ";
    while (!tempStack.empty()) {
        displayText += QString::number(tempStack.top()) + " ";
        tempStack.pop();
    }
    displayText += "\n";

    displayText += "Current Queue (Player Turns): ";
    while (!tempQueue.empty()) {
        displayText += QString::number(tempQueue.front()) + " ";
        tempQueue.pop();
    }
    displayBox->setText(displayText);
}

int main(int argc, char* argv[]) {
    QApplication app(argc, argv);

    //-------------------------------------------main window--------------------------------------------
    QWidget window;
    window.setWindowTitle("Game Level Management");

    // --------------------------------------windows background color----------------------------------------------
    QPalette windowPalette = window.palette();
    windowPalette.setColor(QPalette::Window, Qt::darkGray);
    window.setPalette(windowPalette);
    window.setAutoFillBackground(true);

    QVBoxLayout* layout = new QVBoxLayout(&window);


    QTextEdit* displayBox = new QTextEdit(&window);
    displayBox->setReadOnly(true);
    displayBox->setStyleSheet("background-color: lightgray; color: black; font-size: 14px;");
    layout->addWidget(displayBox);

    // -----------------------------------------------buttons-------------------------------------------------
    QPushButton* pushButton = new QPushButton("Push", &window);
    pushButton->setStyleSheet("background-color: green; color: white; font-size: 14px;");
    QPushButton* popButton = new QPushButton("Pop", &window);
    popButton->setStyleSheet("background-color: red; color: white; font-size: 14px;");
    QPushButton* enqueueButton = new QPushButton("Enqueue", &window);
    enqueueButton->setStyleSheet("background-color: blue; color: white; font-size: 14px;");
    QPushButton* dequeueButton = new QPushButton("Dequeue", &window);
    dequeueButton->setStyleSheet("background-color: orange; color: white; font-size: 14px;");

    layout->addWidget(pushButton);
    layout->addWidget(popButton);
    layout->addWidget(enqueueButton);
    layout->addWidget(dequeueButton);

    int actionCounter = 1;
    int playerCounter = 1;


    QObject::connect(pushButton, &QPushButton::clicked, [&]() {
        pushAction(actionCounter++);
        displayStates(displayBox);
    });

    QObject::connect(popButton, &QPushButton::clicked, [&]() {
        popAction();
        displayStates(displayBox);
    });

    QObject::connect(enqueueButton, &QPushButton::clicked, [&]() {
        enqueuePlayer(playerCounter++);
        displayStates(displayBox);
    });

    QObject::connect(dequeueButton, &QPushButton::clicked, [&]() {
        dequeuePlayer();
        displayStates(displayBox);
    });

//------------------------------------window size------------------------------------
    window.setLayout(layout);
    window.resize(500, 600);
    window.show();

    return app.exec();
}