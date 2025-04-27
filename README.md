[README.md](https://github.com/user-attachments/files/19885517/README.md)

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <map>
#include <cmath>
#include <algorithm>
#include <limits>

using namespace std;

// Düğüm yapısı
struct Node {
    int x, y;
    int g_score;
    int h_score;
    int f_score;
    Node* parent;

    Node(int _x, int _y) : x(_x), y(_y), g_score(numeric_limits<int>::max()), h_score(0), f_score(numeric_limits<int>::max()), parent(nullptr) {}

    bool operator>(const Node& other) const {
        return f_score > other.f_score;
    }
};

// Sezgisel fonksiyon (Manhattan mesafesi)
int heuristic(int x1, int y1, int x2, int y2) {
    return abs(x1 - x2) + abs(y1 - y2);
}

// A* algoritması
vector<pair<int, int>> aStar(vector<vector<int>>& grid, pair<int, int> start, pair<int, int> goal) {
    int rows = grid.size();
    int cols = grid[0].size();

    priority_queue<Node*, vector<Node*>, greater<Node*>> openSet;
    map<pair<int, int>, Node*> allNodes;

    Node* startNode = new Node(start.first, start.second);
    startNode->g_score = 0;
    startNode->h_score = heuristic(start.first, start.second, goal.first, goal.second);
    startNode->f_score = startNode->g_score + startNode->h_score;

    openSet.push(startNode);
    allNodes[start] = startNode;

    int dx[] = {0, 0, 1, -1};
    int dy[] = {1, -1, 0, 0};

    while (!openSet.empty()) {
        Node* current = openSet.top();
        openSet.pop();

        if (current->x == goal.first && current->y == goal.second) {
            vector<pair<int, int>> path;
            Node* temp = current;
            while (temp != nullptr) {
                path.push_back({temp->x, temp->y});
                temp = temp->parent;
            }
            reverse(path.begin(), path.end());
            return path;
        }

        for (int i = 0; i < 4; ++i) {
            int neighborX = current->x + dx[i];
            int neighborY = current->y + dy[i];

            if (neighborX >= 0 && neighborX < cols && neighborY >= 0 && neighborY < rows && grid[neighborY][neighborX] == 0) {
                pair<int, int> neighborPos = {neighborX, neighborY};
                Node* neighbor = allNodes.count(neighborPos) ? allNodes[neighborPos] : new Node(neighborX, neighborY);
                allNodes[neighborPos] = neighbor;

                int tentative_g_score = current->g_score + 1;

                if (tentative_g_score < neighbor->g_score) {
                    neighbor->parent = current;
                    neighbor->g_score = tentative_g_score;
                    neighbor->h_score = heuristic(neighborX, neighborY, goal.first, goal.second);
                    neighbor->f_score = neighbor->g_score + neighbor->h_score;

                    bool inOpenSet = false;
                    priority_queue<Node*, vector<Node*>, greater<Node*>> tempOpenSet = openSet;
                    while (!tempOpenSet.empty()) {
                        if (tempOpenSet.top() == neighbor) {
                            inOpenSet = true;
                            break;
                        }
                        tempOpenSet.pop();
                    }
                    if (!inOpenSet) {
                        openSet.push(neighbor);
                    }
                }
            }
        }
    }

    return {}; // Yol bulunamadı
}

int main() {
    int rows, cols;
    cout << "Izgara boyutunu girin (satır sütun): ";
    cin >> rows >> cols;

    vector<vector<int>> grid(rows, vector<int>(cols, 0));
    cout << "Izgarayı girin (0: boş, 1: engel). Satır satır girin:" << endl;
    for (int i = 0; i < rows; ++i) {
        for (int j = 0; j < cols; ++j) {
            cin >> grid[i][j];
        }
    }

    int startX, startY, goalX, goalY;
    cout << "Başlangıç noktasını girin (satır sütun): ";
    cin >> startY >> startX;
    cout << "Hedef noktasını girin (satır sütun): ";
    cin >> goalY >> goalX;

    pair<int, int> start = {startX, startY};
    pair<int, int> goal = {goalX, goalY};

    if (startX < 0 || startX >= cols || startY < 0 || startY >= rows ||
        goalX < 0 || goalX >= cols || goalY < 0 || goalY >= rows ||
        grid[startY][startX] == 1 || grid[goalY][goalX] == 1) {
        cout << "Geçersiz başlangıç veya hedef noktası." << endl;
        return 1;
    }

    vector<pair<int, int>> path = aStar(grid, start, goal);

    if (!path.empty()) {
        cout << "En kısa yol: ";
        for (const auto& node : path) {
            cout << "(" << node.second << ", " << node.first << ") ";
        }
        cout << endl;
    } else {
        cout << "Hedefe giden bir yol bulunamadı." << endl;
    }

    // Bellek temizleme (basit örnek için atlandı)

    return 0;
}

