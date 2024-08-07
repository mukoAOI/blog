将程序的一部分编译为动态链接库（DLL）有几个主要的原因和优势：

1. **代码复用和模块化**：
   - DLL 允许将功能模块化，使得多个程序可以共享同一个 DLL 文件。这样做有助于减少代码的重复编写，提高开发效率。
   - 如果多个程序需要相同的功能，只需在 DLL 中实现一次即可，程序只需调用 DLL 中的函数即可使用该功能。
2. **资源节约**：
   - 当多个程序共享一个 DLL 时，内存中只需要加载一个 DLL 的实例，这减少了内存的使用量。
   - DLL 中的代码只有在被需要时才会被加载，因此可以降低程序的启动时间。
3. **版本控制和更新**：
   - 如果需要更新功能或修复 bug，只需更新 DLL 文件，而不必重新编译和发布整个程序。
   - 这使得维护和更新变得更加灵活和高效。
4. **动态链接**：
   - DLL 文件允许动态链接，即在运行时才加载和链接到程序中。这使得程序的可执行文件更加轻巧，因为不需要包含所有功能的代码。
5. **分离关注点**：
   - 将某些功能或服务编译为 DLL 可以帮助开发者将关注点分离。程序的主要逻辑可以集中在可执行文件中，而某些通用或特定功能可以在 DLL 中处理。

总之，使用 DLL 可以提高代码的重用性、减少内存占用、简化更新和维护过程，并使程序结构更加模块化和灵活。