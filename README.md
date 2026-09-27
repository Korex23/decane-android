# decane-kit for Android

Decane embedded wallets for native Android apps: sign in with email, phone,
Google or your own tokens, and get EVM, Solana and Tron addresses from one
key. The private key is a 2-of-3 Shamir split that is only ever reconstructed
inside an attested enclave, for the moment it takes to sign.

This repository is where the Android library is published:
**`app.decane:decane-kit`** on GitHub Packages. It is a Kotlin layer over
Decane's shared Rust wallet core (the same core as the Swift SDK,
[DecaneKit](https://github.com/Korex23/decane-swift)), byte-compatible with
the web and React Native SDKs: a wallet made in one opens in the others.

**0.1.0 implements the identity tier:** email, phone, Google-token,
KingsChat-token and custom-token sign-in; wallet creation; provisioning on a
new device; signing for EVM, Solana and Tron. The device tier, passkeys,
browser sign-in and recovery files come next.

## Install

GitHub Packages needs a token even to read a public package. Create a classic
PAT with only `read:packages` and put it in `~/.gradle/gradle.properties`:

```properties
gpr.user=your-github-username
gpr.key=ghp_…
```

```kotlin
// settings.gradle.kts
dependencyResolutionManagement {
    repositories {
        google()
        mavenCentral()
        maven {
            url = uri("https://maven.pkg.github.com/Korex23/decane-android")
            credentials {
                username = providers.gradleProperty("gpr.user").get()
                password = providers.gradleProperty("gpr.key").get()
            }
        }
    }
}

// app/build.gradle.kts
android {
    defaultConfig {
        minSdk = 28
        manifestPlaceholders["decaneRedirectScheme"] = "myapp"
    }
}
dependencies {
    implementation("app.decane:decane-kit:0.1.0")
}
```

## Quick start

```kotlin
val wallet = DecaneWallet.create(
    context,
    DecaneConfig(
        appId = "proj_…",
        apiKey = "dck_live_…",
        chains = listOf("evm:8453", "solana:mainnet"),
        protection = Protection.IDENTITY,
    ),
)

wallet.connectWithEmail("ada@example.com")
val result = wallet.verifyEmailCode("ada@example.com", "123456")
val signature = wallet.signMessage("evm:8453", "hello")
```

## Documentation

- Full spec: https://kit.decane.app/llms-kotlin.txt
- Docs: https://kit.decane.app
- Issues and questions: open an issue here.

MIT licensed.
