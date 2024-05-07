---
title: Aliens Trick 
date: 2024-01-29
math: true

---
As a brief foreword, I think that associating scary names with ideas/techniques such as some of the ones I try to understand here and in later blogs can be a dangerous thing to do. Starting with just the name over the ideas behind the name could turn away potential self-discovery someone could make about these ideas. However, the name itself is probably a necessary thing needed to be concise.

There are other resources on this technique, but I'm writing this to understand an application to a specific problem I found where the intended solution is much simpler and doesn't require this. 

A common model in competitive programming is a "$k$-condition", where exactly or at most "$k$" things may be used. For example, the following [problem](https://cses.fi/problemset/task/2426/) has a pool of $n$ people, where exactly $k$ people must be designated as artists, and $j$ people must be designated as programmers. ($n \leq 10^{5}$, $k \geq 1$, $j \geq 1$, $k + j \leq n$). Each person has a programming and artistic skill ($1 \leq x_{i}, y_{i} \leq 10^{9}$), and we want to maximize the total sum of skills of the people designated to corresponding roles. A person can only be designated to at most one role. In this case, the "$k$-condition" can be either $k$ (the number of artists), $j$ (the number of programmers), or both.

Now, without a "$k$-condition". i.e without the condition on the number of artists (we only need $j$ programmers), this problem becomes significantly easier to solve: Greedily take the maximum $j$ differences (assuming they are greater than 0) between programming skill and artistry skill as programmers, and the rest of the people become artists.  

Aliens trick is a way to eliminate this "$k$-condition" with just an added log factor to our final complexity. Aliens trick requires that the value of our problem is convex/concave in our parameter "$k$". Intuitively, convexity/concavity in one dimension means that as we increase $k$, our optimal value changes by less and less. In this case, as we increase $k$, the best artists with the highest skill will be taken initially, then artists taken later on would be worse and worse for higher $k$, motivating the idea that our optimal value is concave in $k$.

Formally, concavity for a discrete function $f(k)$ for a fixed number of programmers $j$ can be expressed as:
  $f(k) - f(k - 1) \geq f(k + 1) - f(k)$. Where the addition of one more artist adds less and less.

In a convex or concave function, the slope is a monotonically changing value with respect to the parameter $k$. The idea of aliens trick is to binary search on the value of this slope, and find the value of the slope that corresponds to our specific $k$. 

Solving the relaxed problem greedily is equivalent to finding the point on this $f(k)$ function where the derivative $f'(k) = 0$. In Alien's trick problems, this ability to quickly find the point $k$ and value of $f(k)$ when $f'(k) = 0$ is the key step used in every binary search iteration.

Consider the function $g_{\lambda}(k) = f(k) - \lambda k$. This is also a concave function in $k$. This function effectively models our original problem, except that every time we take an additional artist, we also subtract an additional $\lambda$ from our score. This is because we are subtracting $\lambda k$ from our total score, which is equivalent to subtracting $\lambda$ for each indidivdual artist we take. The optimal value of $g_{\lambda}(k)$ can also be computed in a similar way to which we found the optimal solution to the relaxed $f(k)$ problem: we subtract $\lambda$ from ever artist's skill at the start, then run our greedy solution for the original $f(k)$ problem. 

Now, if we have found the point $k$ and value $g_{\lambda}(k)$, where $g_{\lambda}'(k) = 0$, then a little manipulation leads us to see that for this $k$, $g_{\lambda}'(k) = f'(k) - \lambda = 0 \implies f'(k) = \lambda.$ Additionally, we have $f(k) = g_{\lambda}(k) + \lambda k.$ Therefore, binary searching on the slope $f'(k)$ is equivalent to binary searching for this $\lambda$ value in the $g_{\lambda}(k)$ function. Since we can recover the $k$ value in the solution to $g_{\lambda}(k)$ $= 0$, and we also have the $\lambda$ value at this point (given by the binary search midpoint), we can recover $f(k)$ as $g_\lambda(k) + \lambda k$ at this point where $g'_{\lambda}(k) = 0.$ This gives us a relatively quick way to evaluate different values of $f(k)$ additionally, we know if this $k$ value is too large or too small by simply comparing this $k$ with the $k$ given in the input.

In code, the above looks like:

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
using pii = pair<ll, ll>;
#define all(x) x.begin(), x.end()

void solve() {
  ll a, b, n;
  cin >> a >> b >> n;

  vector<ll> x(n), y(n);
  for (int i = 0; i < n; i++) {
    cin >> x[i] >> y[i];
  }

  auto f_a = [&](ll each_a_sub) -> pair<ll, ll> {
    vector<pii> sub_b;
    sub_b.reserve(n);
    ll f = 0;
    for (int i = 0; i < n; i++) {
      ll a_val = x[i] - each_a_sub;
      f += max(0LL, a_val);
      ll diff = y[i] - max(0LL, a_val);
      sub_b.push_back({diff, (a_val <= 0 ? 1 : 0)});
    }
    sort(sub_b.begin(), sub_b.end(), greater<pii>());

    for (int i = 0; i < b; i++) {
      f += sub_b[i].first;
    }
    ll cnt_a = 0;
    for (int i = b; i < n; i++) {
      cnt_a += sub_b[i].second ^ 1;
    }
    return {f, cnt_a};
  };

  ll lo = 0, hi = 1e9;
  while (lo < hi) {
    ll lambda = (lo + hi + 1) >> 1;
    auto [fn, x] = f_a(lambda);
    if (x >= a)
      lo = lambda;
    else
      hi = lambda - 1; // slope is too high
  }

  auto [fn, cnt_a] = f_a(lo);
  cout << fn + a * lo << "\n";
}

int main() {
  cin.tie(0)->sync_with_stdio(false);
  solve();
  return 0;
}
```
Note that the code only did an integer binary search, as opposed to binary searching over real numbers. The proof of how the slope can be contained within the integers can be found in one of the codeforces blogs.

This can be interpreted as an application of using Lagrange multipliers. Our function without the k constraint is easy to solve, and so we add a Lagrange multiplier $\lambda k$ to account for this constraint. Our Lagrangian function (with this Lagrange multiplier added) is still convex, so we can find the optimal values quickly and binary search for the point where the constraint parameter $\lambda$ gives us the $k$ value that we want.

Good problems for this technique:  
- https://atcoder.jp/contests/abc218/tasks/abc218_h
- https://cses.fi/problemset/task/2426/ 


Sources:
- https://serbanology.herokuapp.com/vault/The%20Trick%20From%20Aliens
- https://mamnoonsiam.github.io/posts/attack-on-aliens.html
- https://usaco.guide/adv/lagrange?lang=cpp
- https://codeforces.com/blog/entry/98334
- https://codeforces.com/blog/entry/49691
- https://robert1003.github.io/2020/02/26/dp-opt-wqs-binary-search.html