### [Moving Average from Data Stream](https://leetcode.com/problems/moving-average-from-data-stream)

Difficulty **Easy**

tags `std::queue`

Given a stream of integers and a window size, calculate the moving average of all integers in the sliding window.

For example,
```
MovingAverage m = new MovingAverage(3);
m.next(1) = 1
m.next(10) = (1 + 10) / 2
m.next(3) = (1 + 10 + 3) / 3
m.next(5) = (10 + 3 + 5) / 3
```
很自然地想到用queue.

**solution 1**

```c++
class MovingAverage {
public:
    /** Initialize your data structure here. */
    MovingAverage(int size) : size(size), sum(0.0){
    }

    double next(int val) {
        if (size <= 0) return 0.0;
        if (que.size() < size) {
            sum += val;
            que.push(val);
        } else {
            sum -= que.front();
            sum += val;
            que.pop();
            que.push(val);
        }
        return sum / que.size();
    }
private:
    queue<int> que;
    double sum;
    int size;
};

/**
 * Your MovingAverage object will be instantiated and called as such:
 * MovingAverage obj = new MovingAverage(size);
 * double param_1 = obj.next(val);
 */
```
```c++
// std::queue 支持这样的访问.
myqueue.front() -= myqueue.back();    // 77-16=61

std::cout << "myqueue.front() is now " << myqueue.front() << '\n'; // 61
```
