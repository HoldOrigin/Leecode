# Leecode
刷题笔记
## 1、array 
数组操作也是主要涉及到指针的操作，滑动窗口涉及到双指针，指针之间的距离为窗口的大小
    
### 1、字符数组 char[], 翻转首尾的原因字母;  首尾双指针法， 注意判断首尾指针的边界和相遇问题
    char * reverseVowels(char* s) {
        if(s == nullptr) {
          return s;
        }
        auto istVowels = [string s = "aeiouAEIOU"s](char c) {
            return sting::npos != s.find(c);
        }
        //从索引0 开始
        int n = strlen(s) - 1;
        int start = 0;
        int end = n;
        while (start < end) {
            // 注意边界
            while( start < n && !isVowels(s[start])){
              start++;
            }
            // 注意end的边界 ，判断到元音字母停止循环
            while(end > 0 && !isVowels(s[end])) {
              end--;
            }
            // 判断指针不能相遇，才满足条件
            if (start < end) {
              swap(s[start], s[end]);
              start++;
              end--;
            } 
        }
        return s;
    }
    类似的题型： 翻转字符串中的单词， 单词之间以空格分割， 使用双端队列存储word
    string s reverseWord(string s){
            if (s.empty()) { return s;}
            int left = 0;
            int right = s.size() - 1;
            //首先取出首尾的重复的空格
            while(left < right && s[left] !=' ') left++;
            while(right > left && s[right] != ' ') right--;
            deque<string> dq; // 利用双端队列， 在头部插入元素，在头部取出元素，实现倒序的机制
            // 开始遍历字符串
            while(left < right) {
                char c = s[left];
                string word;
                // 先判断word 满足的条件
                if(word.size() && c ==' ' ){
                    dq.push_front(move(word));
                } else {
                  word += c;
                }
                left++;
            }
            // 最后一个元素末尾没有空格，也需要加入到队列
            dq.push_front(move(word));
            string res;
            // 将队列转换为字符串
            while(!dq.empty()) {
                 res += dq.front();
                 dq.pop_front();
                 // 判断队里是否还有元素， 再加分割符
                if (!dq.empty()) {
                    res +=' ';
                }
            }
            return res;
    }
###  2、滑动窗口
      1、滑动窗口之固定大小
          
      2、滑动窗口之浮动窗口大小

## List  双指针，快慢指针
  ### 1、链表倒序
      ListNode* reverseList(ListNode *head) {
          if (head == nullptr || head->next == nullptr) {
            return nullptr; }
          ListNode* pre = nullptr;
          ListNode* cur = head;
          while(cur) {
            listNode * tmp = cur->next;
            cur->next = pre;
            pre = cur;
            cur = tmp;
          }
          return pre;
      }
      #### 递归倒序， 递归的几个关键点： 1、递归结束的条件 2、进入递归 3、递归的动作
          ListNode *recursive(ListNode *cur, ListNode *pre) {
              //终止条件： 递归到链表结尾， cur == nullptr, 返回翻转后的尾节点
              if (cur == nullptr) {
                  return pre;
              }
              // 递归后继节点
              ListNode * res = recursive(cur->next, cur);
              // cur 节点指回 pre;
              cur->next = pre;
              // 返回翻转后的头结点
              return res;
          }
          ListNode * reverseList(ListNode * head) {
              retrun recusive(head, nullptr);
          }
  ### 2、链表有环， 找到存在换的节
    1、方法 快慢指针
     ListNode * detectCycle(ListNode *head) {
           if(head == nullptr || head->next == nullptr) {
               return nullptr;
           }
           ListNode * slow = head;
           ListNode * fast  = head;
          
           while(fast->next) {
               fast = fast->next->next;
               slow = slow->next;
               //快慢指针相遇，表示有环
               if (fast == slow) {
               
                 //此时a - c = (n-1)(b+c); a 为环入口节点， b = slow 在环中走的距离， C 为剩下环的长度， b+c 为环的长度
                   ListNode  * pos = head;
                   while(pos != slow) {
                     pos = pos->next;
                     slow = slow->next;
                   }
                   return pos;
               }
           }
           return nullptr;
     }
     2、集合统计节点个数
         ListNode * detectCycle(ListNode * head) {
              if(head == nullptr || head->next == nullptr) {
                 return nullptr;
               }
              unordered_set<ListNode*> visited;
              ListNode * cur = head;
              while(cur) {
                // 集合中已经存在元素，重合
                if(visited.count(cur) {
                  return cur;
                }
                visited.insert(cur);
                cur = cur->next;
              }

              return nullptr;
         }

## tree
### dfs 深度优先遍历
    计算Tree的高度
    int depthOfTree(TreeNode * node) {
        // 结束标志到叶子结点
        if (node == nullptr) return 0;
        // 比较节点的左右子树的最大， 需要探索到树的最深处
        return Max(depthOfTree(node->left), depthOfTree(node->right)) + 1;    
    }

### bfs 广度优先遍历
