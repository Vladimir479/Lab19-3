# Lab19-3
#include <stdio.h>
#include <string.h>

typedef struct {
    char title[50];
    int releaseYear;
    char style[20];
    int trackCount;
    int durationMinutes;
} MusicalAlbum;

int writefile(char *fname, MusicalAlbum *albums, int size) {
    FILE *out;
    
    if ((out = fopen(fname, "w")) == NULL) {
        printf("Ошибка открытия файла для записи\n");
        return 0;
    }
    
    // ИЗМЕНЕНИЕ: Убрал пробел между %s и ;
    // Было: "Название: %s ;" 
    // Стало: "Название: %s;"
    for (int i = 0; i < size; i++) {
        fprintf(out, "Название:%s;Год:%d;Стиль:%s;Треков:%d;Длит:%d\n",
                albums[i].title,
                albums[i].releaseYear,
                albums[i].style,
                albums[i].trackCount,
                albums[i].durationMinutes);
    }
    
    fclose(out);
    return 1;
}

int main() {
    MusicalAlbum albums[5];
    
    strcpy(albums[0].title, "The_Dark_Side_of_the_Moon");
    albums[0].releaseYear = 1973;
    strcpy(albums[0].style, "Progressive_Rock");
    albums[0].trackCount = 10;
    albums[0].durationMinutes = 43;
    
    strcpy(albums[1].title, "Thriller");
    albums[1].releaseYear = 1982;
    strcpy(albums[1].style, "Pop");
    albums[1].trackCount = 9;
    albums[1].durationMinutes = 42;
    
    // ... остальные альбомы
    
    // ЗАПИСЬ В ФИКСИРОВАННОМ ФОРМАТЕ
    writefile("albums_fixed.txt", albums, 5);
    
    // ПРОВЕРКА ЧТЕНИЯ SCANF-ОМ
    printf("Проверка чтения scanf-ом:\n");
    printf("=========================\n");
    
    FILE *in = fopen("albums_fixed.txt", "r");
    if (in != NULL) {
        char title[50], style[20];
        int year, tracks, duration;
        
        // Теперь scanf корректно прочитает!
        // Формат: "Название:%s;Год:%d;Стиль:%s;Треков:%d;Длит:%d"
        int result = fscanf(in, "Название:%s;Год:%d;Стиль:%s;Треков:%d;Длит:%d",
                           title, &year, style, &tracks, &duration);
        
        if (result == 5) {
            printf("УСПЕШНО прочитано scanf-ом:\n");
            printf("  Название: %s\n", title);
            printf("  Год: %d\n", year);
            printf("  Стиль: %s\n", style);
            printf("  Треков: %d\n", tracks);
            printf("  Длит: %d\n", duration);
        } else {
            printf("Ошибка чтения scanf-ом! Прочитано %d полей из 5\n", result);
        }
        
        fclose(in);
    }
    
    // Вывод содержимого файла
    printf("\nСодержимое файла:\n");
    printf("=================\n");
    in = fopen("albums_fixed.txt", "r");
    if (in != NULL) {
        char line[200];
        while (fgets(line, sizeof(line), in)) {
            printf("%s", line);
        }
        fclose(in);
    }
    
    return 0;
}


19.3
