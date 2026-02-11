# Android (Kotlin) Code Review Checklist

Use this checklist when reviewing `.kt`, `.java`, or Android layout XML files. Focus on app stability (Crash/ANR), memory management, and modern Kotlin practices.

## 1. Memory Leaks & Lifecycle (Crucial)
- **Context Leaks**: Are `Activity` or `Fragment` contexts passed to long-lived objects (Singletons, static fields, background threads)? Suggest using `applicationContext` or `WeakReference`.
- **Observer Cleanup**: Are `LiveData`, `Flow`, or `BroadcastReceiver` observers properly removed or scoped (e.g., using `viewLifecycleOwner` in Fragments)?
- **View Binding**: In Fragments, is the binding reference set to null in `onDestroyView` to prevent memory leaks?
- **Background Tasks**: Are background tasks (Coroutines, Threads) properly cancelled in `onDestroy` or `onCleared`? Check for misuse of `GlobalScope`.

## 2. Concurrency & ANR Risks
- **Main Thread Blocking**: Are Database (Room), Network (Retrofit), or heavy IO operations performed on the Main Thread? Suggest `Dispatchers.IO`.
- **UI Updates**: Are UI updates explicitly posted to the Main Thread (if not using Main-safe libraries)?
- **Coroutine Scope**: Is `viewModelScope` or `lifecycleScope` used instead of creating custom scopes where possible?

## 3. Kotlin Safety & Best Practices
- **Null Safety**: flag any usage of `!!` (double-bang operator). Suggest `?.` (safe call), `?:` (Elvis operator), or `requireNotNull`.
- **Lateinit**: Is `lateinit var` guarded with `.isInitialized` checks if access is uncertain?
- **Immutability**: Prefer `val` over `var` and `List` over `MutableList` unless mutation is strictly necessary.
- **Data Classes**: Are classes holding data marked as `data class`? Do they override `equals`/`hashCode` if used in `DiffUtil`?

## 4. UI & Performance
- **RecyclerView**: Does the adapter rely on `DiffUtil` for efficient updates? Are `onBindViewHolder` methods lightweight (no object allocation)?
- **Overdraw**: Are there nested layouts with unnecessary `background` colors?
- **Lazy Loading**: Are expensive initializations wrapped in `by lazy`?

## 5. Security (Android Specific)
- **Intents**: Are explicit Intents used for internal component communication? Are pending intents mutable only when necessary?
- **Data Storage**: Is sensitive data stored in `SharedPreferences` without encryption? Suggest `EncryptedSharedPreferences`.