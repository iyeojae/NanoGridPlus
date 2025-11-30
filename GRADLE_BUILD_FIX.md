# EC2에서 빌드 실패 해결 가이드

## 🔧 문제 해결 완료

`gradle.properties` 파일과 `build.gradle` 설정을 업데이트했습니다.

---

## 📥 EC2에서 실행할 명령어

### 1. 최신 코드 가져오기
```bash
cd NanoGridPlus
git pull origin main
```

### 2. JAVA_HOME 설정 (필수)
```bash
# Java 경로 찾기 및 설정
export JAVA_HOME=$(ls -d /usr/lib/jvm/java-17-amazon-corretto* | head -1)
export PATH=$JAVA_HOME/bin:$PATH

# 영구 저장
echo "export JAVA_HOME=\$(ls -d /usr/lib/jvm/java-17-amazon-corretto* 2>/dev/null | head -1)" >> ~/.bashrc
echo "export PATH=\$JAVA_HOME/bin:\$PATH" >> ~/.bashrc
source ~/.bashrc

# 확인
echo "JAVA_HOME: $JAVA_HOME"
java -version
```

### 3. 빌드
```bash
cd NanoGridPlus
./gradlew clean bootJar
```

---

## ✅ 해결된 문제

### 변경 사항:

1. **gradle.properties 추가**
   - Java auto-detect 활성화
   - JVM 메모리 설정
   - 빌드 성능 최적화

2. **build.gradle 수정**
   - Java Toolchain 제거
   - sourceCompatibility/targetCompatibility 직접 지정
   - 더 단순하고 안정적인 설정

---

## 🚀 전체 명령어 (한 번에 실행)

```bash
# EC2에서 실행
cd NanoGridPlus

# 최신 코드
git pull origin main

# JAVA_HOME 설정
export JAVA_HOME=$(ls -d /usr/lib/jvm/java-17-amazon-corretto* | head -1)
export PATH=$JAVA_HOME/bin:$PATH

# 빌드
./gradlew clean bootJar

# 실행
java -jar build/libs/NanoGridPlus-0.0.1-SNAPSHOT.jar --spring.profiles.active=prod
```

---

## 📊 예상 결과

```bash
$ ./gradlew clean bootJar

> Task :clean
> Task :compileJava
> Task :processResources
> Task :classes
> Task :bootJar

BUILD SUCCESSFUL in 45s
7 actionable tasks: 7 executed
```

---

## 🔄 이후 업데이트 시

```bash
# 로컬에서 코드 수정 후
git add .
git commit -m "Update"
git push

# EC2에서
cd NanoGridPlus
git pull
./gradlew clean bootJar
```

완료! 🎉

