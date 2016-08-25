FROM mdeanda/nominatim
MAINTAINER miguel <miguel@thedeanda.com>

# Load initial data
ENV PBF_DATA http://download.geofabrik.de/north-america/us/california-latest.osm.pbf
RUN curl $PBF_DATA --create-dirs -o /app/src/data.osm.pbf
RUN service postgresql start && \
    sudo -u postgres psql postgres -tAc "SELECT 1 FROM pg_roles WHERE rolname='nominatim'" | grep -q 1 || sudo -u postgres createuser -s nominatim && \
    sudo -u postgres psql postgres -tAc "SELECT 1 FROM pg_roles WHERE rolname='www-data'" | grep -q 1 || sudo -u postgres createuser -SDR www-data && \
    sudo -u postgres psql postgres -c "DROP DATABASE IF EXISTS nominatim" && \
    useradd -m -p password1234 nominatim && \
    sudo -u nominatim ./src/utils/setup.php --osm-file /app/src/data.osm.pbf --all --threads 2 && \
    service postgresql stop

EXPOSE 5432
EXPOSE 8080

COPY start.sh /app/start.sh
CMD /app/start.sh
