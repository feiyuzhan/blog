### rules for makefile:
    ```
    target: prereq1 prereq2 
    commands
    ```
1. 每个commands 脚本在tab character之后(用tab键，使用空格会报错)  
2. 如果target的时间戳(写入时间)在prereq之后则表示当前target编译后并没有对prereq进行过修改,这时候会跳过这个target的编译过程并输出: 
   ```
   make: "target" is up to date.
   ```   
3. target依赖于与prereq1和prereq2, 编译器会依次处理prereq, 如果当前处理的 prereq1 同样存在依赖关系，则会递归处理prereq1(先执行 gcc prereq1.o .....) 
4. '--just-print(或-n)' 可以用来要求 make 显示它将为特定工作目标执行的命令，但不要实际执行它们  
5. #是注释, \用于延长当前行





