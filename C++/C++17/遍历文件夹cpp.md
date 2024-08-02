```c++
#include <iostream>
#include <filesystem>
#include <chrono>
using namespace std;
using namespace std::filesystem;


void  hello_file(path ph){
    if (!exists(ph))
        return ;
    else {
        directory_iterator list(ph);
        for (auto &it :list){
            cout<<it.path().filename()<<endl;
            if (is_directory(it))
                continue;
                //hello_file(it);//受否需要遍历
        }
    }
}
int main() {
    auto i =1;
    cout<<i<<endl;
    auto start = std::chrono::high_resolution_clock::now();
    path str("D:\\c\\");//改成需要遍历的地址
    if (!exists(str))        //必须先检测目录是否存在才能使用文件入口.
        return 1;
    hello_file(str);
    auto end = std::chrono::high_resolution_clock::now();
    std::chrono::duration<double, std::milli> float_ms = end - start;

    cout << "elapsed time is " << float_ms.count()
              << " milliseconds" << endl;
    return 0;
}
```