pipeline {
    agent { label 'build_slave'}
   
    
    stages {
        stage('Build-x64') {
            steps {
                git branch: 'tizen', url: 'http://github.sec.samsung.net/chunseok-lee/llvm.git', credentialsId: '2619b682-86fd-41bb-ad68-5277e52d7a7b'
                dir('tools/clang') {
                    git branch: 'release_38', url: 'http://github.sec.samsung.net/chunseok-lee/clang.git', credentialsId: '2619b682-86fd-41bb-ad68-5277e52d7a7b'
                }
                dir('tools/lldb') {
                    git branch: 'tizen30-srr', url: 'http://github.sec.samsung.net/chunseok-lee/lldb.git', credentialsId: '2619b682-86fd-41bb-ad68-5277e52d7a7b'
                }
                sh('[ -e build ] || mkdir build')
                dir('build') {
                    sh '''
                        cmake -DLLDB_DISABLE_CURSES=1 -DLLDB_DISABLE_PYTHON=1 -DLLDB_DISABLE_LIBEDIT=1 -DCMAKE_BUILD_TYPE=Release ..
                        make -j8
                    '''
                }
            }
        }
        stage('Build-rpi2-ubuntu1604') {
            steps {
                git branch: 'tizen', url: 'http://github.sec.samsung.net/chunseok-lee/llvm.git', credentialsId: '2619b682-86fd-41bb-ad68-5277e52d7a7b'
                dir('tools/clang') {
                    git branch: 'release_38', url: 'http://github.sec.samsung.net/chunseok-lee/clang.git', credentialsId: '2619b682-86fd-41bb-ad68-5277e52d7a7b'
                }
                dir('tools/lldb') {
                    git branch: 'tizen30-srr', url: 'http://github.sec.samsung.net/chunseok-lee/lldb.git', credentialsId: '2619b682-86fd-41bb-ad68-5277e52d7a7b'
                }
                
                // we assume that the following docker run is executed :
                //sh('docker run -itd -v /home/twoflower/jenkins-slave/workspace/llvm:/home/llvm --name lldb_build_slave lldb:ubuntu bash')
                sh('docker exec lldb_build_slave /bin/sh -c "mkdir -p /home/llvm/build-rpi2"')
                sh('docker exec lldb_build_slave /bin/sh -c "cd /home/llvm/build-rpi2/; cmake -DLLDB_DISABLE_CURSES=1 -DLLDB_DISABLE_PYTHON=1 -DCMAKE_CROSSCOMPILING=1 -DCMAKE_LIBRARY_ARCHITECTURE=arm-linux-gnueabihf -DCMAKE_C_COMPILER=arm-linux-gnueabihf-gcc -DCMAKE_CXX_COMPILER=arm-linux-gnueabihf-g++ -DLLVM_TABLEGEN=/home/llvm/build/bin/llvm-tblgen -DCLANG_TABLEGEN=/home/llvm/build/bin/clang-tblgen -DLLVM_HOST_TRIPLE=arm-linux-gnueabihf -DLLVM_TARGET_ARCH=ARM -DLLVM_TARGETS_TO_BUILD=ARM -DLLVM_INCLUDE_TESTS=Off -DLLVM_ENABLE_TERMINFO=0 -DLLDB_DISABLE_LIBEDIT=1 ..;"')
                sh('docker exec lldb_build_slave /bin/sh -c "cd /home/llvm/build-rpi2/; make -j8;"')
                
            }
            
        }
    }
  
}
