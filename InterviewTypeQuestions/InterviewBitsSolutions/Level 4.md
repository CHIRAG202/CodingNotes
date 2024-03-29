# Linked List
## 30. Linked List Palindrome
```c++
/*
In O(N) time and O(1) space
Simply go to center of linked list then reverse the right half and then compare by iterating.
Reversing a linked list in O(1) space and O(n) time, if it was doubly linked list O(1) time we can just swap next and prev
*/
Node *cur = head, *next = NULL, *prev = NULL;
tail = head;
while (cur != NULL)
{
    next = cur->next;
    cur->next = prev;
    prev = cur;
    cur = next;
}
head = prev;
```

## 31. Reverse Link List II
https://www.interviewbit.com/problems/reverse-link-list-ii/
```c++
ListNode* Solution::reverseBetween(ListNode* A, int B, int C)
{
    ListNode *cur = A, *start = NULL, *startTemp = NULL, *prev = NULL;
    int length = 0;
    while (length < B)
    {
        length++;
        if (length == B-1) start = cur;
        else if (length == B) startTemp = cur;
        prev = cur;
        cur = cur->next;
    }
    ListNode *end = NULL, *endTemp = NULL, *temp = NULL;
    while (length < C)
    {
        length++;
        temp = cur->next;
        cur->next = prev;
        prev = cur;
        cur = temp;
        if (length == C)
        {
            endTemp = prev;
            end = cur;
            startTemp->next = end;
            if (start != NULL) start->next = endTemp;
            else if (start == NULL) A = endTemp;
        }
    }
    return A;
}
```

## 32. K Reverse Linked List
https://www.interviewbit.com/problems/k-reverse-linked-list/
```c++
ListNode* Solution::reverseList(ListNode* A, int B)
{
    if(A == NULL || B == 1 || B == 0) return A;
    ListNode* head = NULL, *curr = A, *prev = NULL, *tempHead = NULL;
    for(int i = 0; i < B; i++)
    {
        ListNode *temp = curr->next;
        curr->next = prev;
        prev = curr;
        curr = temp;
        if(tempHead == NULL) tempHead = prev;
    }
    head = prev;
    prev = NULL;
    while(curr)
    {
        ListNode* t = NULL;
        for(int i = 0; i < B; i++)
        {
            ListNode *temp = curr->next;
            curr->next = prev;
            prev = curr;
            curr = temp;
            if(t == NULL) t = prev;
        }
        tempHead->next = prev;
        tempHead = t;
    }
    if(tempHead) tempHead->next = NULL;
    return head;
}
```

## 33. Insertion Sort List
```c++
ListNode* Solution::insertionSortList(ListNode* A)
{
    if (A == NULL) return A;
    ListNode *cur = A->next, *prev = A, *head = A;
    ListNode *temp, *check, *checkPrev;
    while (cur != NULL)
    {
        temp = cur->next;
        if (cur->val < prev->val)
        {
            check = head;
            checkPrev = NULL;
            while (check != cur)
            {
                if (check->val > cur->val) break;
                checkPrev = check;
                check = check->next;
            }
            if (checkPrev == NULL)
            {
                head = cur;
                cur->next = check;
                prev->next = temp;
                cur = temp;
            }
            else
            {
                checkPrev->next = cur;
                cur->next = check;
                prev->next = temp;
                cur = temp;
            }
        }
        else
        {
            prev = cur;
            cur = temp;
        }
    }
    return head;
}
```

## 34. Merge Sort List
```c++
ListNode* merge(ListNode* A, ListNode* B)
{
    if (A == NULL) return B;
    if (B == NULL) return A;
    ListNode *head = NULL;
    if (A->val < B->val)
    {
        head = A;
        A = A->next;
    }
    else
    {
        head = B;
        B = B->next;
    }
    ListNode *temp = head;
    while (A != NULL && B != NULL)
    {
        if (A->val < B->val)
        {
            temp->next = A;
            A = A->next;
        }
        else
        {
            temp->next = B;
            B = B->next;
        }
        temp = temp->next;
    }
    if (A != NULL) temp->next = A;
    else temp->next = B;
    return head;
}

ListNode* Solution::sortList(ListNode* A)
{
    ListNode *head = A;
    if (head == NULL || head->next == NULL) return head;
    ListNode *start = A;
    ListNode *end = A->next;
    while (end != NULL && end->next != NULL)
    {
        start = start->next;
        end = (end->next)->next;
    }
    end = start->next;
    start->next = NULL;
    return merge(sortList(head), sortList(end));
}
```

## 35. Reorder List
https://www.interviewbit.com/problems/reorder-list/
```c++
ListNode* Solution::reorderList(ListNode* A)
{
    if (A == NULL || A->next == NULL) return A;
    ListNode *temp, *prev, *mid = A, *cur = A, *newCur, *newHead, *newTemp, *newPrev;
    while (cur != NULL && cur->next != NULL)
    {
        prev = mid;
        mid = mid->next;
        cur = (cur->next)->next;
    }
    prev->next = NULL;
    newCur = mid;
    while (newCur != NULL)
    {
        newTemp = newCur->next;
        if (newCur == mid)
        {
            newPrev = newCur;
            newCur->next = NULL;
            newCur = newTemp;
        }
        else
        {
            newCur->next = newPrev;
            newPrev = newCur;
            newCur = newTemp;
        }
    }
    newHead = newPrev;
    newCur = newHead;
    cur = A;
    while (newCur != NULL && cur != NULL)
    {
        prev = cur;
        newPrev = newCur;
        temp = cur->next;
        newTemp = newCur->next;
        cur->next = newCur;
        if (temp != NULL) newCur->next = temp;
        cur = temp;
        newCur = newTemp;
    }
    return A;
}
```

## 36. List Cycle
https://www.interviewbit.com/problems/list-cycle/
```c++
// Floyd’s Cycle detection
ListNode* Solution::detectCycle(ListNode* A)
{
    if (A == NULL || A->next == NULL) return A;
    ListNode *slow = A, *fast = A;
    while (slow != NULL && fast != NULL)
    {
        slow = slow->next;
        if (fast->next == NULL) return NULL;
        else fast = (fast->next)->next;
        if (slow == fast) break;
    }
    if (slow == NULL || fast == NULL) return NULL;
    ListNode *cur = A;
    while (cur != slow)
    {
        cur = cur->next;
        slow = slow->next;
    }
    return cur;
}
```

# Stacks & Queues
## 37. Largest Rectangle In Histogram
https://www.interviewbit.com/problems/largest-rectangle-in-histogram/
```c++
int Solution::largestRectangleArea(vector<int> &A)
{
    stack<int> st;
    int ans = 0;
    A.push_back(-1);
    for (int i = 0; i < A.size(); ++i)
    {
        while (!st.empty() && A[i] <= A[st.top()])
        {
            int height = A[st.top()];
            st.pop();
            int width = i - (st.empty() ? -1 : st.top()) - 1;
            ans = max(ans, height * width);
        }
        st.push(i);
    }
    A.pop_back();
    return ans;
}
```

## 38. Sliding Window Maximum
https://www.interviewbit.com/problems/sliding-window-maximum/
```c++
vector<int> Solution::slidingMaximum(const vector<int> &A, int k)
{
    deque <int> q;
    vector <int> res;
    int n = A.size();
    for(int i = 0; i < n; ++i)
    {
        while (!q.empty() && q.front() < i-k+1) q.pop_front();
        while (!q.empty() && A[i] > A[q.back()]) q.pop_back();
        q.push_back(i);
        if(i >= k-1) res.push_back(A[q.front()]);
    }
    return res;
}
```

## 39. Min Stack
https://www.interviewbit.com/problems/min-stack/
```c++
stack<int> st;
stack<int> minSt;

MinStack::MinStack()
{
    while (!st.empty()) st.pop();
    while (!minSt.empty()) minSt.pop();
}

void MinStack::push(int x)
{
    st.push(x);
    if (minSt.size() == 0) minSt.push(x);
    else
    {
        if (x <= minSt.top()) minSt.push(x);
        else minSt.push(minSt.top());
    }
}

void MinStack::pop()
{
    if (!st.empty())
    {
        st.pop();
        minSt.pop();
    }
}

int MinStack::top()
{
    if (st.empty()) return -1;
    return st.top();
}

int MinStack::getMin()
{
    if (minSt.empty()) return -1;
    return minSt.top();
}
```