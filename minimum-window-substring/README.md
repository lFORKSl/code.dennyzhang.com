# Leetcode: Minimum Window Substring     :BLOG:Hard:


---

Minimum Window Substring  

---

Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).  

    For example,
    S = "ADOBECODEBANC"
    T = "ABC"
    Minimum window is "BANC".

Note:  
If there is no such window in S that covers all characters in T, return the empty string "".  

If there are multiple such windows, you are guaranteed that there will always be only one unique minimum window in S.  

Blog link: <http://brain.dennyzhang.com/minimum-window-substring>  

Github: [challenges-leetcode-interesting](https://github.com/DennyZhang/challenges-leetcode-interesting/tree/master/minimum-window-substring)  

Credits To: [leetcode.com](https://leetcode.com/problems/minimum-window-substring/description)  

Leave me comments, if you know how to solve.  

    ## Basic Ideas: Two pointers
    ##        Notice: T may have duplicate characters. AABC
    ##                T may be empty or only has one character
    ##              Let's say s[i:j] is the mininum window. Both s[i] and s[j] should be in T
    ##              Pointer: Check s from left to right. If s[k] in T, use pointer2 to find a candidate
    ##              Move pointer2 to find the next candiate
    ##
    ## Complexity: Time ? Space ?
    import copy
    class Solution(object):
        def minWindow(self, s, t):
            """
            :type s: str
            :type t: str
            :rtype: str
            """
            if len(s) == 0 or len(t) == 0:
                return ''
    
            # get frequency for characters in t
            t_m = {}
            for ch in t:
                if ch in t_m:
                    t_m[ch] += 1
                else:
                    t_m[ch] = 1
    
            min_length, min_str = None, ''
            for i in xrange(0, len(s) - len(t)+1):
                ch1 = s[i]
                m = copy.deepcopy(t_m)
                start, end = None, None
                # find the starting point
                if ch1 in m:
                    # print("ch1: %s, i: %d, t:%s" % (ch1, i, t))
                    start, end = i, i
                    m[ch1] -= 1
                    if m[ch1] == 0:
                        del m[ch1]
                    if len(m) != 0:
                        # t has only one character and identity one match
                        # pointer2
                        for j in range(i+1, len(s)):
                            ch2 = s[j]
                            if ch2 in m:
                                m[ch2] -= 1
                                if m[ch2] == 0:
                                    del m[ch2]
                            if len(m) == 0:
                                end = j
                                break
                if len(m) == 0:
                    # find one match
                    if min_length is None or min_length > end-start+1:
                        min_length, min_str = end-start+1, s[start:end+1]
            return min_str
    
    s = Solution()
    print s.minWindow("ADOBECODEBANC", "ABC") # BANC