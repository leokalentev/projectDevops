# ---------- Сборка ----------
FROM gradle:8.5.0-jdk21 AS build

WORKDIR /home/gradle/project

# Копируем проект и сразу ставим владельцем gradle (чтобы gradle мог писать)
COPY --chown=gradle:gradle . .

# Выполняем сборку от пользователя gradle (в образе по умолчанию этот пользователь есть)
USER gradle
# если в проекте есть gradle wrapper, используем ./gradlew; если нет — оставляем gradle
RUN if [ -x ./gradlew ]; then ./gradlew installDist --no-daemon; else gradle installDist --no-daemon; fi

# ---------- Runtime ----------
FROM eclipse-temurin:21-jre
WORKDIR /app
COPY --from=build /home/gradle/project/build/install/demo .
CMD ["./bin/demo"]
