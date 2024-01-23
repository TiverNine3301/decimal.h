PROJECT_NAME = s21_decimal
CC = gcc
OS = $(shell uname)
FLAGS_PLATFORM = $(shell pkg-config --cflags --libs check)
GCOVFLAGS=-fprofile-arcs -ftest-coverage

ifeq ($(OS), Linux)
	FLAGS = -Werror -Wall -Wextra -std=c11
	TEST_FLAGS = -lcheck -pthread -lrt -lm -lsubunit
else
	FLAGS = -Werror -Wall -Wextra -std=c11
	TEST_FLAGS = -lcheck
endif

.PHONY: all clean test s21_decimal.a gcov_report style clang check

all: $(PROJECT_NAME).a
$(PROJECT_NAME).a:
	$(CC) $(FLAGS) -c help.c convert.c math.c other.c compar.c
	ar rcs $(PROJECT_NAME).a help.o convert.o math.o other.o compar.o

test:$(PROJECT_NAME).a
	$(CC) $(FLAGS) test_s21_decimal.c $(PROJECT_NAME).a -o test_s21_decimal $(TEST_FLAGS)
	./test_s21_decimal

check: test
	cppcheck --enable=all --suppress=missingIncludeSystem --inconclusive --check-config *.c *.h
	make test
	leaks -atExit -- test

gcov_report: clean
	$(CC) $(CFLAGS) $(FLAGS_PLATFORM) $(GCOVFLAGS) *.c -o gcov_main
	./gcov_main
	lcov --capture --directory . --output-file coverage.info
	genhtml coverage.info --output-directory gcov_report
	open gcov_report/index.html 

style: clean
	cp ../materials/linters/.clang-format ./
	clang-format -n *.c
	clang-format -n *.h
	rm -rf .clang-format

clang: clean
	cp ../materials/linters/.clang-format ./
	clang-format -i *.c
	clang-format -i *.h
	rm -rf .clang-format

clean:
	rm -rf *.a *.out *.o test_s21_decimal *.html *.css
	rm -rf *.g* *.info report gcov_main gcov_report