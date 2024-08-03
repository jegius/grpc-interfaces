# grpc-interfaces

`grpc-interfaces` — это проект для генерации gRPC интерфейсов на основе файла `.proto` для проекта BookStatistics. Он предоставляет необходимые методы для работы с книгами через gRPC.

## Содержание

- [Описание](#описание)
- [Установка](#установка)
- [Использование](#использование)
- [Публикация через JitPack](#публикация-через-jitpack)
  - [Шаг 1: Подготовьте проект](#шаг-1-подготовьте-проект)
  - [Шаг 2: Создайте файл GitHub репозитория](#шаг-2-создайте-файл-github-репозитория)
  - [Шаг 3: Сборка и публикация проекта](#шаг-3-сборка-и-публикация-проекта)
- [Протокол Buffers - Примеры кода](#протокол-buffers---примеры-кода)

## Описание

Данный проект содержит конфигурацию для генерации gRPC интерфейсов на основе файла `book.proto`, который описывает сервисы и сообщения для работы с книгами.

## Установка

Для установки и настройки проекта выполните следующие шаги:

1. Клонируйте репозиторий:
   ```bash
   git clone https://github.com/Ваша_организация/Ваш_репозиторий.git
   cd Ваш_репозиторий
   ```

2. Убедитесь, что у вас установлена Java 17 и Maven.

3. Соберите проект:
   ```bash
   mvn clean install
   ```

## Использование

Сначала создайте необходимые gRPC интерфейсы, выполнив команду:
```bash
mvn protobuf:compile
```

Затем вы можете импортировать сгенерированные классы в другие проекты, которые будут использовать этот интерфейс для взаимодействия через gRPC.

## Публикация через JitPack

Для публикации вашего проекта с использованием [JitPack](https://jitpack.io/), выполните следующие шаги:

### Шаг 1: Подготовьте проект

Убедитесь, что ваш проект готов к публикации:
- Убедитесь, что файл `pom.xml` корректно настроен.
- Убедитесь, что проект успешно собирается с помощью Maven.

### Шаг 2: Создайте файл GitHub репозитория

Запушьте ваш проект на GitHub:
1. Создайте новый репозиторий на GitHub.
2. Запушьте ваш локальный репозиторий на GitHub:
   ```bash
   git init
   git remote add origin https://github.com/Ваш_пользователь/Ваш_репозиторий.git
   git add .
   git commit -m "Initial commit"
   git push -u origin master
   ```

### Шаг 3: Сборка и публикация проекта

1. Перейдите на [JitPack.io](https://jitpack.io/).
2. Вставьте URL вашего GitHub репозитория в поле поиска и нажмите "Lookup".
3. Выберите версию для сборки (например, тег или ветку) и нажмите "Get it".
4. JitPack создаст и опубликует вашу библиотеку, предоставив вам инструкции по добавлению зависимости в ваш проект.

Добавьте зависимость в ваш `pom.xml` другого проекта:

```xml
<dependency>
    <groupId>com.github.Ваш_пользователь</groupId>
    <artifactId>Ваш_репозиторий</artifactId>
    <version>Текущая_версия</version>
</dependency>
```

## Протокол Buffers - Примеры кода

Файл `book.proto` содержит описание gRPC сервиса и сообщений для работы с книгами:

```proto
syntax = "proto3";

option java_multiple_files = true;
package com.deps.bookstatistics;

service BookService {
  rpc addBook (BookRequest) returns (BookResponse) {}
}

message BookRequest {
  string title = 1;
  string author = 2;
  int32 totalPages = 3;
  string coverImage = 4;
}

message BookResponse {
  string id = 1;
  string title = 2;
  string author = 3;
  int32 totalPages = 4;
  string coverImage = 5;
}
```

Для генерации Java классов из этого файла, используйте плагин `protobuf-maven-plugin`:

```xml
<build>
    <extensions>
        <extension>
            <groupId>kr.motd.maven</groupId>
            <artifactId>os-maven-plugin</artifactId>
            <version>1.7.0</version>
        </extension>
    </extensions>
    <plugins>
        <plugin>
            <groupId>org.xolstice.maven.plugins</groupId>
            <artifactId>protobuf-maven-plugin</artifactId>
            <version>0.6.1</version>
            <configuration>
                <protocArtifact>com.google.protobuf:protoc:3.17.3:exe:${os.detected.classifier}</protocArtifact>
                <pluginId>grpc-java</pluginId>
                <pluginArtifact>io.grpc:protoc-gen-grpc-java:${grpc.version}:exe:${os.detected.classifier}</pluginArtifact>
                <protoSourceRoot>${project.basedir}/src/main/proto</protoSourceRoot>
            </configuration>
            <executions>
                <execution>
                    <goals>
                        <goal>compile</goal>
                        <goal>compile-custom</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

Эти настройки позволяют вам автоматически генерировать Java классы для gRPC интерфейсов при сборке вашего проекта с помощью Maven.

---

Для получения актуальной информации об использовании JitPack посетите [официальную документацию](https://jitpack.io/docs/).

