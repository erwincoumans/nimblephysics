FROM keenon/diffdart:base

# This is allowed to be empty string, but if it's not it must be prefixed by
ARG VERSION
ARG GIT_HASH

RUN mkdir /wheelhouse

# Build Python 3.8

ENV PYTHON="/opt/python/cp38-cp38/bin/python3.8"
ENV PATH="/opt/python/cp38-cp38/bin/:${PATH}"
ENV PYTHON_VERSION="cp38-cp38"
ENV PYTHON_INCLUDE="/opt/python/cp38-cp38/include/python3.8/"
ENV PYTHON_LIB="/opt/python/cp38-cp38/lib/python3.8"
ENV PYTHON_VERSION_NUMBER="3.8"

# Install pybind11
ENV CPATH="${PYTHON_INCLUDE}"
RUN git clone https://github.com/pybind/pybind11.git && \
    pushd pybind11 && \
    mkdir build && \
    pushd build && \
    cmake .. && \
    make install -j10
# Install pytest
RUN ${PYTHON} -m pip install pytest
RUN ${PYTHON} -m pip install auditwheel
RUN git clone https://github.com/keenon/nimblephysics.git
RUN cd nimblephysics && \
    git checkout ${GIT_HASH} && \
    cat setup.py && \
    ${PYTHON} setup.py sdist bdist_wheel && \
    ${PYTHON} -m auditwheel repair dist/nimblephysics-${VERSION}-${PYTHON_VERSION}-linux_x86_64.whl
RUN mv /nimblephysics/wheelhouse/nimblephysics-${VERSION}-${PYTHON_VERSION}-manylinux2_17_x86_64.whl /wheelhouse