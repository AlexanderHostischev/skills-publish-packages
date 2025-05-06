# Лабораторна робота 8 завдання 1.2

## Курс "Publish Packages" був успішно пройдений:

Файл **publish.yml**:

```yml
name: Publish to Docker
on:
  push:
    branches:
      - main
permissions:
  packages: write
  contents: read
jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      # Add your test steps here if needed...
      - name: Docker meta
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ghcr.io/AlexanderHostischev/publish-packages/game
          tags: type=sha
      - name: Login to GHCR
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.repository_owner }}
          password: ${{ secrets.GITHUB_TOKEN }}
      - name: Build container
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
```

Після цього в нас опублікувався образ проекту: https://github.com/AlexanderHostischev/skills-publish-packages/pkgs/container/publish-packages%2Fgame

<img width="780" alt="Screenshot 2025-05-06 at 21 51 59" src="https://github.com/user-attachments/assets/3c813512-a7c3-4b2d-95d5-cf6955a49d82" />

Запустимо образ на локальній машині:

<img width="601" alt="Screenshot 2025-05-06 at 21 52 35" src="https://github.com/user-attachments/assets/b38147c9-3774-4472-b6b5-6b15a2d0d36f" />
<img width="602" alt="Screenshot 2025-05-06 at 21 52 59" src="https://github.com/user-attachments/assets/84265c0a-786d-4af9-ad27-ee230efb27d2" />

Після цього запустимо команду:
```bash
docker run -dp 8080:80 --rm ghcr.io/alexanderhostischev/publish-packages/game:sha-ae55475
```
І перейдемо до браузеру за посиланням http://localhost:8080:

<img width="973" alt="Screenshot 2025-05-06 at 21 55 07" src="https://github.com/user-attachments/assets/d3147f0a-d745-4d80-b6d0-04315e84ed3d" />

Як ми бачимо застосунок працює.
