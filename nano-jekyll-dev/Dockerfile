# Cribbed from https://github.com/woodie/nanoserver-ruby

# Base Image
FROM microsoft/nanoserver AS base

# Core Image
FROM microsoft/windowsservercore AS core

ENV RUBY_MAJOR 2.3
ENV RUBY_VERSION 2.3.3
ENV DEVKIT_VERSION 4.7.2
ENV DEVKIT_BUILD 20130224-1432

RUN mkdir C:\\tmp

ADD https://dl.bintray.com/oneclick/rubyinstaller/rubyinstaller-${RUBY_VERSION}-x64.exe C:\\tmp
RUN C:\\tmp\\rubyinstaller-%RUBY_VERSION%-x64.exe /silent /dir="C:\Ruby" /tasks="assocfiles,modpath"

ADD https://dl.bintray.com/oneclick/rubyinstaller/DevKit-mingw64-64-${DEVKIT_VERSION}-${DEVKIT_BUILD}-sfx.exe C:\\tmp
RUN C:\\tmp\\DevKit-mingw64-64-%DEVKIT_VERSION%-%DEVKIT_BUILD%-sfx.exe -o"C:\DevKit" -y

RUN setx PATH %PATH%;C:\DevKit\bin;C:\Ruby\bin -m
RUN echo - C:\\Ruby > config.yml
RUN ruby C:\\devkit\\dk.rb install
RUN gem install bundler

# Final Image
FROM base

COPY --from=core C:\\Ruby C:\\Ruby
COPY --from=core C:\\DevKit C:\\DevKit
RUN setx PATH %PATH%;C:\DevKit\bin;C:\Ruby\bin -m
RUN echo - C:\\Ruby > config.yml
RUN ruby C:\\devkit\\dk.rb install

ADD . C:\\blog
WORKDIR C:\\blog
RUN bundle install

EXPOSE 80
ENTRYPOINT [ "c:\\blog\\runme.bat" ]
